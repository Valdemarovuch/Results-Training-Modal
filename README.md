# 🎯 Results-Training-Model: PFM-1 Detection

[![YOLO](https://img.shields.io/badge/YOLO-v11-blue.svg)](https://github.com/ultralytics/ultralytics)
[![TensorRT](https://img.shields.io/badge/TensorRT-FP16-76B900.svg)](https://developer.nvidia.com/tensorrt)
[![Python](https://img.shields.io/badge/Python-3.10+-yellow.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](https://opensource.org/licenses/MIT)

Цей репозиторій містить результати навчання, скрипти інференсу та оптимізовані ваги моделі комп'ютерного зору для автоматизованого виявлення вибухонебезпечних предметів (мін типу ПФМ-1 "Пелюстка") в умовах зниженої візуальної інформативності.

## 📌 Про проєкт

Традиційні алгоритми детекції часто дають збій при роботі з малими об'єктами асиметричної форми, які зливаються з фоном. У цьому проєкті реалізовано:
- **Anchor-Free архітектуру** на базі YOLOv11.
- **Параметричну оптимізацію Loss-функцій** (Box Loss = 15.0, Cls Loss = 0.5) для кращої локалізації дрібних об'єктів.
- **Апаратну оптимізацію (Hardware Inference)** за допомогою NVIDIA TensorRT та квантування у формат FP16.
- **Інтеграцію SAHI** (Slicing Aided Hyper Inference) для обробки зображень високої роздільної здатності.

## 📊 Результати навчання (Metrics)

Модель навчалася на датасеті обсягом 10-13 тис. зображень (аугментація: Mosaic, Mixup, Color Jittering). Після 300 епох навчання були досягнуті наступні результати:

| Метрика | Значення | Примітка |
|---------|----------|----------|
| **Precision** | 98.85% | Мінімізація хибних спрацювань |
| **Recall** | 96.26% | Здатність знайти всі об'єкти |
| **mAP@0.5** | **99.19%** | Загальна точність детекції |
| **mAP@0.5:0.95** | 89.43% | Висока точність меж (Bounding Box) |

### 📈 Графіки навчання (Training Metrics)

Нижче наведено загальний графік навчання за 300 епох (зменшення Loss-функцій та зростання mAP/Precision/Recall):

![Training Results](metrics/results.png)

**Матриця помилок (Normalized Confusion Matrix):**
Цей графік демонструє високу впевненість моделі у правильній класифікації мін ПФМ-1 та мінімальну кількість хибних спрацювань на елементи фону (Background).

![Confusion Matrix](metrics/confusion_matrix_normalized.png)

### ⚡ Продуктивність (GPU RTX 4060)
- **Базовий інференс (PyTorch):** 58.4 мс (~17 FPS)
- **Оптимізований інференс (TensorRT FP16):** **23.7 мс (~42 FPS)**

## 🗂 Структура репозиторію

```text
Results-Training-Model/
├── data/               # Скрипти для завантаження та препроцесингу датасету
├── models/             # Збережені ваги моделі (.pt, .onnx, .engine)
├── notebooks/          # Jupyter notebooks з графіками Loss та аналізом метрик
├── scripts/            # Python-скрипти для інференсу та конвертації в TensorRT
├── requirements.txt    # Залежності проєкту
└── README.md
