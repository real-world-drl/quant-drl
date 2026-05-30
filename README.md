# Quantization of DRL Models for Embedded Microcontrollers

Project landing page for the **ICRA 2026** paper
*Quantization of DRL Models for Embedded Microcontrollers* — a toolchain and
firmware for running Deep Reinforcement Learning (DRL) actor policies on
microcontroller-scale hardware (ESP32-S3), controlling a low-cost quadrupedal
robot using only proprioception and on-board inference.

> P. Böhm, A. C. Chapman, P. Moghadam, P. Pounds, and J. J. Chung,
> *"Quantization of DRL Models for Embedded Microcontrollers"*,
> 2026 IEEE International Conference on Robotics and Automation (ICRA),
> Vienna, Austria, June 2026.

## What the paper is about

DRL policies are expensive to run — typically needing a CPU/GPU and a
framework runtime that won't fit on a microcontroller. This work makes DRL
actor networks deployable on MCU-class devices by:

- proposing a **streamlined actor network** optimized for inference-only
  deployment and integer quantization, for both **SAC** and **TD3**;
- adding a **GRU-based recurrent encoder** via a custom,
  quantization-compatible implementation (standard ONNX `GRU` does not import
  cleanly into the embedded runtime);
- quantizing both to **int8** and deploying them with **ESP-DL** on an
  **ESP32-S3**, driving a real quadrupedal robot from on-board inference.

## Companion repositories

The code is split across focused repositories so each stage stands alone:

| Repo | What it does |
|------|--------------|
| [**esp-dl-quant-icra2026**](https://github.com/real-world-drl/esp-dl-quant-icra2026) | Quantization toolchain: trained TorchScript actor → ONNX → ESP-DL `.espdl` int8 model. CPU-only, end to end. Ships the trained QuaidSIM-v4 actors and calibration data. |
| [**esp-dl-inference-icra2026**](https://github.com/real-world-drl/esp-dl-inference-icra2026) | ESP-IDF firmware that runs the quantized `.espdl` actor on an ESP32-S3. Receives observations over MQTT, runs ESP-DL inference on-device, publishes actions over MQTT. |
| [**quaid-sim-cpp**](https://github.com/real-world-drl/quaid-sim-cpp) | Quadruped simulator that can drive the inference firmware over the documented MQTT protocol. |

*More repositories will be linked here as they are published.*

## Where to start

- **Reproduce the quantized models** → start with
  [esp-dl-quant-icra2026](https://github.com/real-world-drl/esp-dl-quant-icra2026)
  (`TorchScript in, .espdl out`).
- **Run a model on hardware** → flash
  [esp-dl-inference-icra2026](https://github.com/real-world-drl/esp-dl-inference-icra2026)
  to an ESP32-S3.
- **Just want to quantize a Python-trained `nn.GRU` for ESP-DL?** → see
  [`GRU_QUICKSTART.md`](https://github.com/real-world-drl/esp-dl-quant-icra2026/blob/main/GRU_QUICKSTART.md)
  in the quantization repo.

## Citation

If you use this work in academic research, please cite the ICRA 2026 paper.
The canonical BibTeX entry (provisional until the DOI and proceedings details
are finalized post-publication) lives in
[esp-dl-quant-icra2026](https://github.com/real-world-drl/esp-dl-quant-icra2026#citation):

```bibtex
@inproceedings{bohm2026quantization,
  author    = {B{\"o}hm, Peter and Chapman, Archie C. and Moghadam, Peyman and Pounds, Pauline and Chung, Jen Jen},
  title     = {Quantization of {DRL} Models for Embedded Microcontrollers},
  booktitle = {2026 IEEE International Conference on Robotics and Automation (ICRA)},
  year      = {2026},
  month     = jun,
  address   = {Vienna, Austria},
  publisher = {IEEE},
  url       = {https://2026.ieee-icra.org/},
  note      = {To appear. DOI and pages will be added post-publication.}
}
```

## Authors

Peter Böhm¹˒², Archie C. Chapman¹, Peyman Moghadam², Pauline Pounds¹, and
Jen Jen Chung¹

¹ School of Electrical Engineering and Computer Science, The University of
Queensland, Australia
² CSIRO Robotics, DATA61, CSIRO, Australia

## License

MIT.
