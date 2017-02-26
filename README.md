# java-计算器
package Computer;  
  
import java.awt.BorderLayout; 一种布局格式 
import java.awt.Color;  颜色的设定
import java.awt.Container;  内容面板
import java.awt.Font;  字体
import java.awt.GridLayout;  一种布局管理器
import java.awt.event.ActionEvent;  监听事项
import java.awt.event.ActionListener;  监听器
import java.util.Stack;  
  
import javax.swing.JApplet;  基本框架
import javax.swing.JButton;  按钮
import javax.swing.JFrame;  容器
import javax.swing.JPanel;  面板容器
import javax.swing.JTextField;  文本框
  
public class Jianyi extends JApplet implements ActionListener  （ 定义大致面板）
{ 
    private static final long serialVersionUID = 1L;  
    private JTextField textField = new JTextField("");  一个私有的文本框
    String operator = "";//操作  
    String input = "";//输入的 式子  
    boolean flag =  true;  //定义一个布尔类型的变量，初始值是true；布尔类型只有两个值，false 和 true。如果变量值为 0 就是 false，否则为 true,布尔变量只有这两个值。
    public void init()//覆写Applet里边的init方法  
    {  
    	//整体的框架是内容面板C，分为两个内容，分别是文本框部分（textfield）,以及按钮部分
        Container C = getContentPane();//获取内容面板，因为JFrame不能直接添加组件，需要用getContentPane()函数获取内容面板，再在内容面板上进行添加组件 
        JButton b[] = new JButton[16];  //添加16个按钮
        JPanel panel = new JPanel();  //创建一个面板，并且采用 BorderLayout布局格式
        C.add(textField, BorderLayout.NORTH);//1.文本框 2.布局格式3.位置  
        C.add(panel,BorderLayout.CENTER);//将panel放置到中间  
        panel.setLayout(new GridLayout(4,4,5,5));  //四行，五列，水平间距，垂直间距
        String name[]={"7","8","9","+","4","5","6","-","1","2","3","*","0","C","=","/"};//设置 按钮  
        for(int i=0;i<16;i++)//添加按钮  
        {  
            b[i] = new JButton(name[i]);  
			b[i].setBackground(new Color(255,200,0));  //public final static Color orange = new Color(255,200,0);背景颜色
            b[i].setForeground(Color.black);//数字键 设置为红颜色 ，字体颜色 
            if(i%4==3)  
                b[i].setForeground(Color.RED);  
            b[i].setFont(new Font("宋体",Font.PLAIN,16));//设置字体格式  
            panel.add(b[i]);  将该按钮添加到面板中
            b[i].addActionListener(this);  
        }  
        b[13].setForeground(Color.blue);//非数字键，即运算键设置为红颜色  
        b[13].setForeground(Color.blue);
        b[14].setForeground(Color.red);
    }  
	public void actionPerformed(ActionEvent e)   （说明当按下某一个按钮时，文本框上显示的内容）
    {  
        int cnt = 0;
        String actionCommand = e.getActionCommand();  //返回按钮对象
        if(actionCommand.equals("+")||actionCommand.equals("-")||actionCommand.equals("*") ||actionCommand.equals("/"))               
        	input +=" "+actionCommand+" ";  
        else if(actionCommand.equals("C"))  
            input = "";  
        else if(actionCommand.equals("="))//当监听到等号时，则处理 input  
        {  
            input+= "="+compute(input);  
            textField.setText(input);  
            input="";  
            cnt = 1;  
        }  
        else  
            input += actionCommand;//数字为了避免多位数的输入 不需要加空格  
        if(cnt==0)  //只要没有输入国等号，则文本框不会清零
        textField.setText(input);  
    }  
    private String compute(String input) （进行等号之后的运算）
    {  
        String str[];  
        str = input.split(" ");  //降str这个字符串用空格“ ”进行分割 ， 分割后的字符串数组放在a[]中比如 111,222,333 那么a[0]=111 a[1]=222 a[2]=333
        Stack<Double> s = new Stack<Double>();  
        double m = Double.parseDouble(str[0]);  
        s.push(m);  //s.push_back将a逐个按入栈的顺序放入s中
        for(int i=1;i<str.length;i++)  
        {  
            if(i%2==1) //如果到了符号位   
            {    
                if(str[i].compareTo("+")==0)   //如果字符串和加号比为零，即如果字符就是加号 
                {    
                    double help = Double.parseDouble(str[i+1]);    //help为符号位的下一位
                    s.push(help);    //将help的值放入栈中
                }    
                    
                if(str[i].compareTo("-")==0)    //如果字符为减号
                {    
                    double help = Double.parseDouble(str[i+1]); help为符号位的下一位
                    s.push(-help);   将-help按次序放入栈中
                } 
                    
                if(str[i].compareTo("*")==0)    如果字符为乘号
                {   
                    double help = Double.parseDouble(str[i+1]);  help为符号位的下一位 
                    double ans = s.peek();//取出栈顶元素   即栈顶元素的和为ans
                    s.pop();//消栈   
                    ans=help*ans;    
                    s.push(ans);    将ans放入栈中
                }    
                    
                if(str[i].compareTo("/")==0)    同上乘号部分
                {
                    double help = Double.parseDouble(str[i+1]);  
                    double ans = s.peek();    
                    s.pop();    
                    ans/=help;    
                    s.push(ans);    
                }    
            }    
        }    
        double ans = 0d;  ans 的值为零
        while(!s.isEmpty())    如果栈不是空的
        {    
            ans+=s.peek(); //加上栈顶元素   
            s.pop();    //消栈
        }    
        String result = String.valueOf(ans);  结果就等于ans
        return result;  函数的返回值为ans
    }  
    public static void main(String args[])  
    {  
        JFrame frame = new JFrame("张舒嵘-计算器");  
        Jianyi applet = new Jianyi();  
        frame.getContentPane().add(applet, BorderLayout.CENTER);  
        applet.init();//applet的init方法  
        applet.start();//线程开始  
        frame.setSize(200, 200);//设置窗口大小  
        frame.setVisible(true);//设置窗口可见  
    }  
  
}  
