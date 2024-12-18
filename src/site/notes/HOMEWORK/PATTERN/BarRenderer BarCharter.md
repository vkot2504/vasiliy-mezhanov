---
{"dg-publish":true,"permalink":"/homework/pattern/bar-renderer-bar-charter/"}
---

``` python
from PIL import Image, ImageDraw
import os
import re
import tempfile
import abc


def main():
    pairs = [("Mon", 16), ("Tue", 17), ("Wed", 19), ("Thu", 22),
             ("Fri", 24), ("Sat", 21), ("Sun", 19)]
    textBarRenderer = TextBarRenderer()
    textBarRenderer.render("Forecast 6/8", pairs)

    imageBarRenderer = ImageBarRenderer()
    imageBarRenderer.render("Forecast 6/8", pairs)


class BarRenderer(metaclass=abc.ABCMeta):
    """Абстрактный класс для рендеринга гистограммы."""

    @abc.abstractmethod
    def initialize(self, bars, maximum):
        pass

    @abc.abstractmethod
    def draw_caption(self, caption):
        pass

    @abc.abstractmethod
    def draw_bar(self, name, value):
        pass

    @abc.abstractmethod
    def finalize(self):
        pass

    def render(self, caption, pairs):
        """Общий метод для рендеринга гистограммы."""
        maximum = max(value for _, value in pairs)
        self.initialize(len(pairs), maximum)
        self.draw_caption(caption)
        for name, value in pairs:
            self.draw_bar(name, value)
        self.finalize()


class TextBarRenderer(BarRenderer):
    """Рендерер для текстовой гистограммы."""

    def __init__(self, scaleFactor=40):
        self.scaleFactor = scaleFactor

    def initialize(self, bars, maximum):
        assert bars > 0 and maximum > 0
        self.scale = self.scaleFactor / maximum

    def draw_caption(self, caption):
        print("{0:^{2}}\n{1:^{2}}".format(caption, "=" * len(caption),
                                          self.scaleFactor))

    def draw_bar(self, name, value):
        print("{} {}".format("*" * int(value * self.scale), name))

    def finalize(self):
        pass


class ImageBarRenderer(BarRenderer):
    """Рендерер для графической гистограммы."""

    COLORS = ["red", "green", "blue", "yellow", "magenta", "cyan"]

    def __init__(self, stepHeight=10, barWidth=30, barGap=2):
        self.stepHeight = stepHeight
        self.barWidth = barWidth
        self.barGap = barGap

    def initialize(self, bars, maximum):
        assert bars > 0 and maximum > 0
        self.index = 0
        self.image = Image.new("RGB", (bars * (self.barWidth + self.barGap),
                                       maximum * self.stepHeight), "white")
        self.draw = ImageDraw.Draw(self.image)

    def draw_caption(self, caption):
        self.filename = os.path.join(tempfile.gettempdir(),
                                     re.sub(r"\W+", "_", caption) + ".png")

    def draw_bar(self, name, value):
        color = ImageBarRenderer.COLORS[self.index % len(ImageBarRenderer.COLORS)]
        width, height = self.image.size
        x0 = self.index * (self.barWidth + self.barGap)
        x1 = x0 + self.barWidth
        y0 = height - (value * self.stepHeight)
        y1 = height - 1
        self.draw.rectangle([x0, y0, x1, y1], fill=color)
        self.index += 1

    def finalize(self):
        self.image.save(self.filename)
        print("Wrote", self.filename)
