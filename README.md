# manim-
#我的第一个库
from manimlib.imports import *
import os
import pyclbr
#导入包

#创建一个类（形状，该类继承Scens属性）
class Shapes(Scene):
    #A few simple shapes
    #Python 2.7 version runs in Python 3.7 without changes
    def construct(self):
       #定义了一个默认的圆
        circle = Circle()
         #定义了一个默认的正方形
        square = Square()
        #定义了一条直线，从点[3,0,0]到[5,0,0]
        line=Line(np.array([3,0,0]),np.array([5,0,0]))
        #定义了一个三角形，从点[0,0,0],到[1,1,0]，[0,0,0]
        triangle=Polygon(np.array([0,0,0]),np.array([1,1,0]),np.array([1,-1,0]))

        #以showcreation的方式绘制圆
        self.play(ShowCreation(circle))
        #以淡出的方式绘制圆
        self.play(FadeOut(circle))
        #以中心扩张的方式绘制方形
        self.play(GrowFromCenter(square))
        #将方形变成三角形，最终三角形放在屏幕上。
        self.play(Transform(square,triangle))
         #将线段添加到画布（场景中）
        self.add(line)

class MoreShapes(Scene):
    #A few more simple shapes
    #2.7 version runs in 3.7 without any changes
    #Note: I fixed my 'play command not found' issue by installing sox
    def construct(self):
        circle = Circle(color=PURPLE_A)
        square = Square(fill_color=GOLD_B, fill_opacity=1, color=GOLD_A)
        square.move_to(UP+LEFT)
        circle.surround(square)
        rectangle = Rectangle(height=2, width=3)
        ellipse=Ellipse(width=3, height=1, color=RED)
        ellipse.shift(2*DOWN+2*RIGHT)
        pointer = CurvedArrow(2*RIGHT,5*RIGHT,color=MAROON_C)
        arrow = Arrow(LEFT,UP)
        arrow.next_to(circle,DOWN+LEFT)
        rectangle.next_to(arrow,DOWN+LEFT)
        ring=Annulus(inner_radius=.5, outer_radius=1, color=BLUE)
        ring.next_to(ellipse, RIGHT)

        self.add(pointer)
        self.play(FadeIn(square))
        self.play(Rotating(square),FadeIn(circle))
        self.play(GrowArrow(arrow))
        self.play(GrowFromCenter(rectangle), GrowFromCenter(ellipse), GrowFromCenter(ring))

class MovingShapes(Scene):
    #Show the difference between .shift() and .move_to
    def construct(self):
        circle=Circle(color=TEAL_A)
        circle.move_to(LEFT)
        square=Circle()
        square.move_to(LEFT+3*DOWN)

        self.play(GrowFromCenter(circle), GrowFromCenter(square), rate=5)
        self.play(ApplyMethod(circle.move_to,RIGHT), ApplyMethod(square.shift,RIGHT))
        self.play(ApplyMethod(circle.move_to,RIGHT+UP), ApplyMethod(square.shift,RIGHT+UP))
        self.play(ApplyMethod(circle.move_to,LEFT+UP), ApplyMethod(square.shift,LEFT+UP))

class AddingText(Scene):
    #Adding text on the screen
    def construct(self):
        my_first_text=TextMobject("Writing with manim is fun")
        second_line=TextMobject("and easy to do!")
        second_line.next_to(my_first_text,DOWN)
        third_line=TextMobject("for me and you!")
        third_line.next_to(my_first_text,DOWN)

        self.add(my_first_text, second_line)
        self.wait(2)
        self.play(Transform(second_line,third_line))
        self.wait(2)
        second_line.shift(3*DOWN)
        self.play(ApplyMethod(my_first_text.shift,3*UP))
        ###Try uncommenting the following###
        #self.play(ApplyMethod(second_line.move_to, LEFT_SIDE-2*LEFT))
        #self.play(ApplyMethod(my_first_text.next_to,second_line))


class AddingMoreText(Scene):
    #Playing around with text properties
    def construct(self):
        quote = TextMobject("Imagination is more important than knowledge")
        quote.set_color(RED)
        quote.to_edge(UP)
        quote2 = TextMobject("A person who never made a mistake never tried anything new")
        quote2.set_color(YELLOW)
        author=TextMobject("-Albert Einstein")
        author.scale(0.75)
        author.next_to(quote.get_corner(DOWN+RIGHT),DOWN)

        self.add(quote)
        self.add(author)
        self.wait(2)
        self.play(Transform(quote,quote2),ApplyMethod(author.move_to,quote2.get_corner(DOWN+RIGHT)+DOWN+2*LEFT))
        self.play(ApplyMethod(author.scale,1.5))
        author.match_color(quote2)
        self.play(FadeOut(quote))

class RotateAndHighlight(Scene):
    #Rotation of text and highlighting with surrounding geometries
    def construct(self):
        square=Square(side_length=5,fill_color=YELLOW, fill_opacity=1)
        label=TextMobject("Text at an angle")
        label.bg=BackgroundRectangle(label,fill_opacity=1)
        label_group=VGroup(label.bg,label)  #Order matters
        label_group.rotate(TAU/8)
        label2=TextMobject("Boxed text",color=BLACK)
        label2.bg=SurroundingRectangle(label2,color=BLUE,fill_color=RED, fill_opacity=.5)
        label2_group=VGroup(label2,label2.bg)
        label2_group.next_to(label_group,DOWN)
        label3=TextMobject("Rainbow")
        label3.scale(2)
        label3.set_color_by_gradient(RED, ORANGE, YELLOW, GREEN, BLUE, PURPLE)
        label3.to_edge(DOWN)

        self.add(square)
        self.play(FadeIn(label_group))
        self.play(FadeIn(label2_group))
        self.play(FadeIn(label3))

class BasicEquations(Scene):
    #A short script showing how to use Latex commands
    def construct(self):
        eq1=TextMobject("$\\vec{X}_0 \\cdot \\vec{Y}_1 = 3$")
        eq1.shift(2*UP)
        eq2=TexMobject(r"\vec{F}_{net} = \sum_i \vec{F}_i")
        eq2.shift(2*DOWN)

        self.play(Write(eq1))
        self.play(Write(eq2))
