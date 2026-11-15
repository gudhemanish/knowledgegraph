# Setup

Recorded from the environment that produced `logs/metaqa_n100_seeds123.txt`.

| Item | Value |
| --- | --- |
| Date | 15 November 2026 |
| Machine | Local Windows PC, not Google Colab |
| Operating system | Windows 10.0.26300 |
| Python | 3.15.0 (`D:\Python315\python.exe`) |
| GPU | NVIDIA GeForce RTX 3050 Ti Laptop GPU, 4096 MiB, driver 621.24 |
| Model runtime | Ollama `llama3.2:latest`, 3.0 GB, 100% GPU, context 8192 |
| Working directory | `KG-GPT-main/metaqa` |

Package versions are the output of `python -m pip freeze` on that interpreter, saved in `requirements.txt`. The freeze lists every package installed in that interpreter, including packages this project does not import.

The scored run used the same 100 questions per hop, drawn with sample seed 42, and generation seeds 1, 2, and 3:

```
python -u metaqa_1hop_test.py --model llama3.2 --sample 100 --seed 42 --gen-seeds 1,2,3
python -u metaqa_2hop_test.py --model llama3.2 --sample 100 --seed 42 --gen-seeds 1,2,3
python -u metaqa_3hop_test.py --model llama3.2 --sample 100 --seed 42 --gen-seeds 1,2,3
```
