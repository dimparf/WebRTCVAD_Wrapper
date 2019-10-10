# WebRTCVAD Wrapper

Это простая обёртка на Python для [WebRTC](https://webrtc.org/) Voice Activity Detection (VAD). Поддерживается только Python 3.

[VAD](https://en.wikipedia.org/wiki/Voice_activity_detection) - это детектор голосовой активности, который позволяет удалять тишину/извлекать фрагменты с речью (или другими звуками) из wav аудиозаписи.

**WebRTCVAD_Wrapper** упрощает работу с WebRTC VAD: избавляет пользователя от необходимости самому извлекать фреймы/кадры из аудиозаписи и снимает ограничения на параметры обрабатываемой аудиозаписи.

## Установка

Данная обёртка имеет следующие зависимости: [pydub](https://github.com/jiaaro/pydub) и [py-webrtcvad](https://github.com/wiseman/py-webrtcvad).

Установка с помощью pip:
```
pip install git+https://github.com/Desklop/WebRTCVAD_Wrapper
```

## Использование

1. Из вашего кода Python:
```python
from webrtcvad_wrapper import WebRTCVAD

vad = WebRTCVAD()

audio = vad.read_wav('test.wav')
filtered_segments = vad.filter(audio)

segments_with_voice = [filtered_segment[1] for filtered_segment in filtered_segments if filtered_segment[0]]
for j, segment in enumerate(segments_with_voice):
    path = 'segment1_%002d.wav' % (j + 1)
    print("Сохранение '%s'" % (path))
    vad.write_wav(path, segment)
```

Класс [WebRTCVAD]() содержит следующие методы:
- `read_wav()`: загрузка .wav аудиозаписи и приведение её в поддерживаемый формат
- `write_wav()`: сохранение найденных фрагментов в .wav аудиозапись
- `get_frames()`: извлечение фреймов с необходимым смещением
- `filter_frames()`: обработка вывода от `webrtcvad.Vad().is_speech()`
- `filter()`: объединяет `get_frames()` и `filter_frames()`
- `set_mode()`: установка чувствительности WebRTC VAD

2. Как инструмент командной строки:
```
python3 -m webrtcvad_wrapper.cli input.wav output.wav
```
Где:
- `input.wav` - имя исходного .wav аудиозаписи
- `output.wav` или `output` - шаблонное имя для .wav аудиозаписей, в которые будут сохранены найденные фрагменты с речью/звуком в формате `output_%i.wav`

В данном варианте используются следующие параметры:
- WebRTC VAD с уровнем чувствительности/"агрессивности" 3
- длина фрейма 10 миллисекунд
- фрагмент считается фрагментом с речью/звуком, если он содержит более 90% фреймов, в которых webrtcvad нашёл речь/звук

---

Если у вас возникнут вопросы или вы хотите сотрудничать, можете написать мне на почту: vladsklim@gmail.com или в [LinkedIn](https://www.linkedin.com/in/vladklim/).