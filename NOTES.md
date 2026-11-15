# Troubleshooting record

These are the three failures that occurred while making this checkout runnable. Each one happened, and the outcome below is what followed. The 500-question MetaQA run in `logs/metaqa.txt` then exited 0, so no further fix was applied to that run.

## 1. Missing FactKG test pickle

Command: `python data/preprocess.py --factkg_test` with the README example path.

Outcome: `FileNotFoundError` for `factkg/data/factkg_test.pickle`. That path was a placeholder.

Fix: downloaded the official FactKG archive and extracted `factkg_test.pickle`.

Result: preprocess wrote 9,041 claims to `factkg/extracted_test_set.jsonl`.

## 2. Windows pickle read of the full DBpedia graph

Command: `python data/make_type_dict.py --kg .\data\dbpedia_2015_undirected.pickle`.

Outcome: `OSError [Errno 22] Invalid argument` inside `pickle.load`. The file is about 5.9 GB, and Windows rejects a single pickle read of that size.

Fix: load the pickle through a chunked reader. The full graph still does not fit in 16 GB of RAM, so later FactKG use went to `dbpedia_2015_undirected_light.pickle`.

Result: the light graph loads. The MetaQA evaluation does not use this DBpedia file. It uses `metaqa/data/metaqa_kg.pickle`.

## 3. OpenAI client blocked by Application Control

Command: a FactKG inference call that imported the OpenAI package.

Outcome: `DLL load failed while importing jiter: An Application Control policy has blocked this file` (`jiter.cp315-win_amd64.pyd`). `Unblock-File` did nothing because the file had no Zone.Identifier mark. The same policy blocks the unsigned PyTorch `c10.dll`.

Fix: inference calls were pointed at a local Llama 3.2 model served by Ollama at `http://127.0.0.1:11434`, instead of the OpenAI client.

Result: `llama3.2` loaded at 100% GPU. The MetaQA command in `SETUP.md` then completed, and the full output is in `logs/metaqa.txt`.
