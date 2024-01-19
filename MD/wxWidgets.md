## 常用类
### 应用程序类wxApp
>必需、且仅有一个实例

其类应该继承自wxApp
```C++
public myApp : public WxApp
```
其内必须有函数OnInit代替main作为入口，返回bool代替int
```C++
public myApp : public WxApp{
public:
    bool OnInit(){

    }
}
```
为了启动该对象，需要这样一行语句
```C++
IMPLEMENT_APP(MyApp)
```
### 窗口类wxFrame
创造一个窗口

其类应当继承自wxFrame
```C++
public MyFrame : public wxFrame
```
#### wxFrame()
调用的父类构造函数参数如下
```C++
wxFrame::wxFrame(
    wxWindow* parent,
    wxWindowID id,
    const wxString& title,
    const wxPoint& pos = wxDefaultPosition,
    const wxSize& size = wxDefaultSize,
    long style = DEFAULT_FRAME_STYLE,
    const wxString& name = wxFrameNameStr
)
```
1. 父窗口<br>如果确定了父窗口，则其会被父窗口连带最小化和还原<br>但该项通常是nullptr
2. 窗口标识符<br>尽量使用值为负数的宏wxID_ANY表示默认
3. 窗口标题<br>传入一个字符串
4. 窗口位置<br>传入一个wxPoint对象<br>直接调用构造函数wxPoint(int x, int y)传入临时对象<br>使用默认宏wxDefaultPosition
5. 窗口大小<br>传入一个wxSize对象<br>直接调用构造函数wxSize(int width, int height)传入临时对象<br>使用默认宏wxDefaultSize
6. 风格<br>在此列举一些常用风格<br>wxDEFAULT_FRAME_STYLE，推荐使用，包含标题、最大最小化、关闭、大小可调<br>wxCAPTION和wxSYSTEM_MENU强绑定以显示标题栏<br>wxMINIMIZE_BOX绑定标题栏，显示最小化按钮<br>wxMAXIMIZE绑定标题栏，绑定可变窗口，显示最大化按钮<br>wxCLOSE_BOX绑定标题栏，显示关闭按钮<BR>wxRESIZE_BORDER使窗口可拖拽改变大小（可变窗口）<br>wxSTAY_ON_TOP使窗口在最上层
7. 窗口名字（不知用处）
#### wxFrame::Centre函数
使窗口在屏幕中心
```C++
void Centre(int direction = wxBOTH)
```
wxBOTH是一个指定水平和竖直都居中的宏<br>如有需要可以使用宏wxHORIZONTAL表示水平居中，使用宏wxVERTICAL表示竖直居中
#### bool Create()
创建窗口，参数和构造函数一致
### 菜单wxMenu
若想显示一个菜单，需要构造菜单和菜单条两种对象，并需要将菜单挂载到菜单条上

菜单可以有多个菜单项
#### wxMenu()
无参数
#### wxMenu::Append函数
用来为菜单添加一个菜单项

返回一个菜单项wxMenuItem的指针

参数列表
```C++
wxMenuItem* Append(
    int id,
    const wxString& item = wxEmptyString,
    const wxString& helpString = wxEmptyString,
    wxItemKind kind = wxITEM_NORMAL
)
```
1. id是一个标识（在同一个窗口内具有唯一性）<br>一般地，我们使用对应作用的宏来为其赋值
2. item是一个字符串形式的简短描述，并允许在描述后以形同“\tCTRL+c”的形式附加快捷键
3. helpString是一个字符串形式的详细说明
4. kind默认为普通菜单
### 菜单条wxMenuBar
#### wxMenuBar()
一般使用默认构造函数
```C++
wxMenuBar(long style = 0)
```

有参构造可以生成一个带有初始菜单的菜单条
```C++
wxMenuBar(
    size_t n,
    wxMenu* menus[],
    const wxString titles[],
    long style = 0
)
```
1. n是菜单数量
2. menus是菜单类的数组首地址
3. titles是菜单们的标题数组
4. 风格默认
#### wxMenuBar::Append函数
给菜单条添加一个菜单（尾插）

返回一个布尔值代表插入成功/失败

参数列表
```C++
bool Append(
    wxMenu* menu,
    const wxString& title
)
```
1. menu是要添加的菜单的指针
2. title是该菜单的名字
#### wxFrame::SetMenuBar函数
这个函数将一个菜单条挂载到某个窗口上；注意该函数的对象是wxFrame
```C++
void SetMenuBar(wxMenuBar* menuBar)
```
### 文件对话框wxFileDialog
该对话框可以选择文件，一般用于选择文件打开、保存
>需求wx/dirdlg.h头文件

要使用文件对话框，需要在对应的事件处理函数中生成一个该类对象（一般选择在栈上生成），然后使用ShowModel()函数显示
#### wxFileDialog()
参数列表
```C++
wxFileDialog(
    wxWindow* parent,
    const wxString& message = wxFileSelectPromptStr,
    const wxString& defaultDir = wxEmptyString,
    const wxString& defaultFile = wxEmptyString,
    const wxString wildcard = wxFileSelectorDefaultWildcardStr,
    long style = wxFD_DEFAULT_STYLE,
)
```
1. parent是该对话框的父窗口指针，一般采用this实现
2. message是该对话框显示的信息
3. defaultDir默认目录信息
4. defaultFile默认文件信息
5. wildcard通配符，有形同 GIF files (*.gif) | *.gif的形式
6. style风格。通过宏调用已有格式<br>wxFD_DEFAULT_STYLE：同wxFD_OPEN<br>wxFD_OPEN：“打开”<br>wxFD_SAVE：“保存”<br>wx_FD_FILE_MUST_EXIST：文件必须存在才能被选择<br>wxFD_OVERWRITE_PROMPT：提示覆盖
#### wxFileDialog::ShowModel()
函数标签
```C++
virtual int wxFileDialog::ShowModel()
```
若用户选择完毕，点击按钮，则返回wxID_OK；否则返回wxID_CANCEL

一般塞进if中直接进行接下来的操作
### 文件夹对话框wxDirDialog
该对话框可以选择文件夹
>需求wx/dirdlg.h头文件
#### wxDirDialog()
参数列表
```C++
wxDirDialog(
    wxWindow* parent,
    const wxString& message = wxDirSelectorPromptStr,
    const wxString& defaultPath = wxEmptyString,
    long style = wxDD_DEFAULT_STYLE
)
```
1. parent是该对话框的父窗口指针，一般采用this实现
2. message是该对话框显示的信息
3. defaultPath默认文件路径
4. style风格。通过宏调用已有格式<br>wxDD_DEFAULT_STYLE：等效于wxDEFAULT_DIALOG_STYLE | wxRESIZE_BORDER<br>wxDEFAULT_DIALOG_STYLE：等效于wxCAPTION | wxSYSTEM_MENU | wxCLOSE_BOX<br>wxDD_DIR_MUST_EXIST：仅允许使用现有文件夹（即不会出现“创建新目录按钮”）
#### wxDirDialog::ShowModel()
函数标签
```C++
virtual int wxDirDialog::ShowModel()
```
若用户选择完毕，点击按钮，则返回wxID_OK；否则返回wxID_CANCEL
#### wxDirDialog::GetPath()
函数标签如下
```C++
virtual wxString wxDirDialog::GetPath()const
```
返回用户选择的路径
### 文件类wxFile
处理文件内容
>需求wx/file.h头文件
#### 枚举OpenMode
枚举了读取文件的方式
```C++
enum OpenMode{
    read,
    write,
    read_write,
    write_append,
    write_excl
}
```
1. read：只读
2. write：全删后写入
3. read_write：可读可写
4. write_append：追加写入
5. write_excl：安全写入（不存在则创建）
#### wxFlie()
可以无参构造

有参构造以某种方式打开某文件
```C++
wxFile(const wxString& filename,
OpenMode mode = wxFile::read
)
```

#### wxFile::Access函数
尝试是否能以某个模式打开文件
```C++
static bool wxFile::Access(
    const wxString& name,
    wxFile::OpenMode mode
)
```
1. name是文件名
2. mode是枚举的打开方式
#### wxFile::Open函数
打开文件
```C++
bool wxFile::Open(
    const wxString& filename,
    wxFile::OpenMode mode = wxFile::read,
    int access = wxS_DEFAULT
)
```
1. filename：文件名
2. mode是读文件模式，默认只读
3. access是新文件的访问权限。<br>宏wxS_DEFAULT默认允许所有人读取、写入
#### wxFile::Create函数
创建用于写入的文件
```C++
bool wxFile::Create(
    const wxString& filename,
    bool overwrite = false,
    int access = wxS_DEFAULT
)
```
1. filename：文件名
2. overwrite复写，默认不覆盖，若设为true则覆盖同名文件
3. access是新文件的访问权限。<br>宏wxS_DEFAULT默认允许所有人读取、写入
#### wxFile::ReadAll函数
读取文件全部内容为字符串
```C++
bool wxFile::ReadAll(
    wxString* str,
    const wxMBConv& conv = wxConvAuto()
)
```
第二个参数是默认编码
#### wxFile::Write函数
将字符串写入文件
```C++
bool wxFile::Write(
    const wxString& s,
    const wxMBConv& = wxConvUTF8
)
```
第二个参数大多数时候没有意义
### 文件名类wxFileName
处理文件自身属性
>需求wx/filename.h头文件
#### wxFileName::Assign函数
从某种格式的路径创建文件
参数列表
```C++
void wxFileName::Assign(
    const wxString& fullpath,
    wxPathFormat format = wxPATH_NATIVE
)

void wxFileName::Assign(
    const wxString& path,
    const wxString& name,
    wxPathFormat format = wxPATH_NATIVE
)
```
1. fullpath全路径
2. path目标路径（不包括创建文件本身）
3. name文件名
4. format：路径格式的值，主要是路径分隔符。<br>宏wxPATH_NATIVE指定当前平台的格式
### 文件输出流wxFileOutputStream
流输出到文件
>需求wx/wfstream.h头文件
#### wxFileOutputStream()
参数列表
```C++
wxFileOutputStream::wxFileOUtputStream(const wxString& ofilename)
```
ofilename是全地址（路径+文件名）

！：应使用wxStreamBase::IsOk()验证流是否成功
#### wxFileOutputStream::GetFile函数
返回文件
```C++
wxFile* wxFileOutputStream::GetFile()const
```
## 常用宏
### 窗口标识符
|名称|描述|
|-|-|
|wxID_ANY|自动产生一个值为负数的ID|
|wxID_LOWEST|最小系统标识符4999|
|wxID_HIGEST|最大系统标识符5999|
|wxID_OPEN|打开文件|
|wxID_CLOSE|关闭窗口|
|wxID_NEW|新建窗口/新建文档|
|wxID_SAVE|保存文件|
|wxID_SAVEAS|另存为|
## 事件处理
### 事件表
1. 只有继承wxEvtHandler的类才能使用（如wxWindow，wxMenu等）
2. 在该类内使用DECLARE_EVENT_TABLE()声明我需要使用事件表
3. 在文件末尾用BEGIN_EVENT_TABLE起头，END_EVENT_TABLE结尾实现事件表
4. 其中开始事件表时需要两个参数，一是使用该事件表的类，二是其父类
5. 在事件表内部用宏添加事件——函数映射。宏的参数是事件接收者的ID和该事件的处理函数的函数名
6. 事件处理函数必须返回空，且接收一个事件引用
```C++
BEGIN_EVENT_TABLE(Myframe, wxFrame)
    EVT_MENU(wxID_NEW, Myframe::OnNew)//这里的宏是EVT_MENU表示菜单使用
END_EVENT_TABLE()

//其中被调用函数参数应当为
void OnOpen(wxCommandEvent& e);
```
