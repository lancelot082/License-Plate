# License-Plate

## Обнаружение объектов с использованием YOLOv8

Этот проект реализует задачу **детектирования объектов** с использованием модели **YOLOv8** на кастомном датасете. Вся подготовка, визуализация и обучение выполнены на базе библиотеки [Ultralytics YOLO](https://docs.ultralytics.com/).

---

### Структура данных

Проект использует датасет с аннотациями в формате YOLO:
- Изображения хранятся по путям:
  - `dataset/images/train`
  - `dataset/images/val`
- Аннотации (разметка bounding box'ов):
  - `dataset/labels/train`
  - `dataset/labels/val`
![image](https://github.com/user-attachments/assets/3ca251fd-9059-4867-8e5c-a8f3f2a22040)
  
Каждый `.txt` файл содержит строки в формате:  
`<class_id> <x_center> <y_center> <width> <height>`  
*(все координаты нормализованы: от 0 до 1)*

---

### Визуализация разметки

Код позволяет отобразить bounding box'ы на изображении:
- Считываются аннотации из `.txt` файлов.
- Нормализованные координаты преобразуются в пиксели.
- С помощью OpenCV изображение отображается с рамками и подписями классов.

---

### Обучение модели YOLOv8

Модель обучается с помощью CLI-интерфейса `ultralytics`:

```bash
yolo task=detect mode=train model=yolov8m.pt data=dataset.yaml epochs=20 imgsz=640 batch=16 save=True evolve=True
```

Пояснение параметров:
- `model=yolov8m.pt` — используется средняя по размеру модель YOLOv8.
- `data=dataset.yaml` — конфигурационный файл с путями к данным и описанием классов.
- `epochs=20` — количество эпох обучения.
- `imgsz=640` — размер входного изображения.
- `batch=16` — размер батча.
- `evolve=True` — автоматический подбор гиперпараметров.

---

### Валидация модели

```bash
yolo task=detect mode=val model=yolov8m.pt data=dataset.yaml save_json=True
```

- Производится оценка точности на валидационном наборе.
- Результаты сохраняются в COCO-совместимом формате (`save_json=True`).

---

### Предсказания на новых данных

```bash
yolo task=detect mode=predict model=yolov8m.pt source=dataset/images/test save_txt=True save_conf=True save_crop=True
```
![video5_0](https://github.com/user-attachments/assets/cf7d3479-d9d4-427d-9bfd-f8af6b177638)
![video6_870](https://github.com/user-attachments/assets/5dc72f8a-6309-446f-a894-4de02b0b2bc3)

- Модель выполняет предсказание на новых изображениях.
- Сохраняются:
  - `.txt` с результатами (`save_txt=True`)
  - Уверенность (`save_conf=True`)
  - Кропы объектов (`save_crop=True`)
