---
{"dg-publish":true,"permalink":"/bridge/"}
---

**ФИО:** 2022-ФГиИБ-ИБ-1б Воронов Даниил Алексеевич 

```python
from abc import abstractmethod, ABCMeta
import os
import re
import tempfile
from PIL import Image, ImageDraw

class BarRenderer(metaclass=ABCMeta):
    @abstractmethod
    def render_caption(self, caption):
        pass

    @abstractmethod
    def render_bar(self, *args):
        pass

class TextBarRenderer(BarRenderer):
    def __init__(self, scaleFactor=40):
        self.scaleFactor = scaleFactor

    def render_caption(self, caption):
        print("{0:^{2}}\n{1:^{2}}".format(caption, "=" * len(caption), self.scaleFactor))

    def render_bar(self, name, value, scale):
        print("{} {}".format("*" * int(value * scale), name))

class ImageBarRenderer(BarRenderer):
    COLORS = ("red", "green", "blue", "yellow", "magenta", "cyan")
    
    def __init__(self, stepHeight=10, barWidth=30, barGap=2):
        self.stepHeight = stepHeight
        self.barWidth = barWidth
        self.barGap = barGap

    def create_image(self, width, height):
        return Image.new("RGB", (width, height), color="white")

    def render_caption(self, caption):
        pass

    def render_bar(self, draw, x0, y0, x1, y1, color):
        draw.rectangle([x0, y0, x1, y1], fill=color)

class BarCharter:
    def __init__(self, renderer: BarRenderer):
        self.__renderer = renderer

    def render(self, caption, pairs):
        maximum = max(value for _, value in pairs)
        scale = self.__renderer.scaleFactor / maximum if isinstance(self.__renderer, TextBarRenderer) else 1

        self.__renderer.render_caption(caption)

        if isinstance(self.__renderer, TextBarRenderer):
            for name, value in pairs:
                self.__renderer.render_bar(name, value, scale)
        elif isinstance(self.__renderer, ImageBarRenderer):
            width = len(pairs) * (self.__renderer.barWidth + self.__renderer.barGap)
            height = maximum * self.__renderer.stepHeight
            image = self.__renderer.create_image(width, height)
            draw = ImageDraw.Draw(image)

            for index, (name, value) in enumerate(pairs):
                color = ImageBarRenderer.COLORS[index % len(ImageBarRenderer.COLORS)]
                x0 = index * (self.__renderer.barWidth + self.__renderer.barGap)
                x1 = x0 + self.__renderer.barWidth
                y0 = height - (value * self.__renderer.stepHeight)
                y1 = height
                self.__renderer.render_bar(draw, x0, y0, x1, y1, color)

            filename = os.path.join(tempfile.gettempdir(), re.sub(r"W+", "_", caption) + ".png")
            image.save(filename)
            print("Изображение сохранено по данному пути:", filename)

def main():
    pairs = (("Mon", 16), ("Tue", 17), ("Wed", 19), ("Thu", 22),
             ("Fri", 24), ("Sat", 21), ("Sun", 19))

    textBarCharter = BarCharter(TextBarRenderer())
    textBarCharter.render("Forecast 6/8", pairs)

    imageBarCharter = BarCharter(ImageBarRenderer())
    imageBarCharter.render("Forecast 6/8", pairs)

if __name__ == "__main__":
    main()
```

