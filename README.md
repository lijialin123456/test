# test
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.textinput import TextInput
from kivy.uix.image import AsyncImage
import folium
import webbrowser

class Toolbar(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.orientation = 'horizontal'
        self.size_hint_y = None
        self.height = 50

        self.add_widget(Label(text='My App'))
        self.add_widget(Button(text='Button 1', on_press=self.open_website))
        self.add_widget(Button(text='Button 2'))
        self.add_widget(Button(text='Button 3', on_press=self.go_to_login))

    def open_website(self, instance):
        webbrowser.open('https://blog.csdn.net/u011897062/article/details/136217730')

    def go_to_login(self, instance):
        # 添加登录界面到布局中
        self.parent.parent.add_widget(LoginScreen())

class LoginScreen(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        self.orientation = 'vertical'

        self.add_widget(Label(text='User Name'))
        self.username = TextInput(multiline=False)
        self.add_widget(self.username)

        self.add_widget(Label(text='Password'))
        self.password = TextInput(password=True, multiline=False)
        self.add_widget(self.password)

        # 添加“登录”和“返回”按钮
        login_button = Button(text='log on', on_press=self.login)
        back_button = Button(text='black', on_press=self.back)
        self.add_widget(login_button)
        self.add_widget(back_button)

    def login(self, instance):
        # 这里添加登录逻辑的代码
        print('log on')

    def back(self, instance):
        app_root = App.get_running_app().root
        app_root.clear_widgets()
        app_root.add_widget(MyBoxLayout())

class MyBoxLayout(BoxLayout):
    def __init__(self, **kwargs):
        super(MyBoxLayout, self).__init__(**kwargs)
        self.orientation = 'vertical'
        
        self.add_widget(Label(text='Welcome to My App'))
        self.add_widget(Button(text='Click me'))
        self.add_widget(Button(text='Click me to login', on_press=self.go_to_login))

        # 初始化输入框，但不显示
        self.username = TextInput(multiline=False)
        self.password = TextInput(password=True, multiline=False)

    def click_me(self, *args):
        print('Button Clicked')

    def go_to_login(self, *args):
        self.clear_widgets()
        self.add_widget(Label(text='User Name'))
        self.add_widget(self.username)
        self.add_widget(Label(text='Password'))
        self.add_widget(self.password)

class MyApp(App):
    def build(self):
        layout = BoxLayout(orientation='vertical')
        
        label = Label(text='My App')
        layout.add_widget(label)
        
        image = AsyncImage(source='222.png')
        layout.add_widget(image)

        toolbar = Toolbar()
        layout.add_widget(toolbar)

        # 在主页面中添加地图
        # 创建一个基本地图对象
        m = folium.Map(location=[35.47563950114027, 139.60489599411562], zoom_start=10)

        # 在地图上添加标记
        folium.Marker(
            location=[37.7749, -122.4194],
            popup='San Francisco',
            icon=folium.Icon(color='blue')
        ).add_to(m)

        # 保存地图为 HTML 文件
        m.save('map.html')

        # 在浏览器中打开地图文件
        webbrowser.open('map.html')

        return layout

if __name__ == '__main__':
    MyApp().run()
