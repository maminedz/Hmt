using System;							
using System.Collections.Generic;							
using System.ComponentModel;							
using System.Drawing;							
using System.IO.Ports;							
using System.Linq;							
using System.Text.RegularExpressions;							
using System.Threading;							
using System.Windows.Forms;							
namespace HeadTracker_App;							
public class Form1 : Form							
{							
	private static SerialPort serialPort1 = new SerialPort();						
	private static double[] Move = new double[2];						
	private static double[] Scale = new double[2];						
	private static bool Connect = false;						
	private static bool Start = false;						
	private static byte MouseMode = 0;						
	private Thread Updater = new Thread(Update);						
	private static Bitmap bmp;						
	private static Pen p;						
	private static Graphics g;						
	private static int W = Screen.PrimaryScreen.Bounds.Width;						
	private static int H = Screen.PrimaryScreen.Bounds.Height;						
	private IContainer components;						
	private ComboBox comboBox1;						
	private ComboBox comboBox2;						
	private Button button1;						
	private PictureBox pictureBox1;						
	private Timer timer1;						
	private Button button2;						
	private Button button3;						
	private Button button4;						
	private ComboBox comboBox3;						
	private ComboBox comboBox4;						
	private ComboBox comboBox5;						
	private Timer timer2;						
	public Form1()						
	{						
		InitializeComponent();					
	}						
	private void Form1_Load(object sender, EventArgs e)						
	{						
		Updater.Start();					
		((ListControl)comboBox2).SelectedIndex = 5;					
		ComboBox obj = comboBox3;					
		int selectedIndex = (((ListControl)comboBox4).SelectedIndex = 10);					
		((ListControl)obj).SelectedIndex = selectedIndex;					
		((ListControl)comboBox5).SelectedIndex = 0;					
	}						
	private void Form1_FormClosing(object sender, FormClosingEventArgs e)						
	{						
		WriteData(100, 0);					
		WriteData(100, 0);					
		WriteData(100, 0);					
		if (serialPort1.IsOpen)					
		{					
			serialPort1.Close();				
		}					
		if (Updater.IsAlive)					
		{					
			Updater.Abort();				
		}					
	}						
	private void comboBox1_Click(object sender, EventArgs e)						
	{						
		comboBox1.Items.Clear();					
		foreach (string item in new List<string>(SerialPort.GetPortNames()).Distinct().ToList())					
		{					
			comboBox1.Items.Add((object)("COM" + Regex.Replace(item, "\\D", "")));				
		}					
	}						
	private void button1_Click(object sender, EventArgs e)						
	{						
		if (!Connect)					
		{					
			if (((Control)comboBox1).Text != "")				
			{				
				serialPort1.PortName = comboBox1.SelectedItem.ToString();			
				serialPort1.BaudRate = Convert.ToInt32(((Control)comboBox2).Text);			
				serialPort1.DataBits = 8;			
				serialPort1.StopBits = (StopBits)1;			
				serialPort1.Parity = (Parity)0;			
				try			
				{			
					serialPort1.Open();		
				}			
				catch (Exception)			
				{			
					Connect = false;		
				}			
				if (serialPort1.IsOpen)			
				{			
					Connect = true;		
					timer1.Start();		
					timer2.Start();		
					MessageBox.Show(serialPort1.PortName + " 연결에 성공하였습니다.", "알림", (MessageBoxButtons)0, (MessageBoxIcon)64);		
				}			
				else			
				{			
					Connect = false;		
					MessageBox.Show(serialPort1.PortName + " 연결에 실패하였습니다.", "알림", (MessageBoxButtons)0, (MessageBoxIcon)16);		
				}			
			}				
			else				
			{				
				MessageBox.Show("통신포트를 선택하십시오", "알림", (MessageBoxButtons)0, (MessageBoxIcon)48);			
			}				
		}					
		else					
		{					
			try				
			{				
				serialPort1.Close();			
			}				
			catch (Exception)				
			{				
				Connect = true;			
			}				
			if (!serialPort1.IsOpen)				
			{				
				Connect = false;			
				timer1.Stop();			
				timer2.Stop();			
				MessageBox.Show(serialPort1.PortName + " 연결을 해제하였습니다.", "알림", (MessageBoxButtons)0, (MessageBoxIcon)64);			
			}				
			else				
			{				
				Connect = true;			
				MessageBox.Show(serialPort1.PortName + " 연결 헤제를 실패하였습니다.", "알림", (MessageBoxButtons)0, (MessageBoxIcon)16);			
			}				
		}					
		((Control)button1).Text = (Connect ? "연결해제" : "연결하기");					
		ComboBox obj = comboBox1;					
		bool enabled = (((Control)comboBox2).Enabled = !Connect);					
		((Control)obj).Enabled = enabled;					
		Button obj2 = button2;					
		Button obj3 = button3;					
		Button obj4 = button4;					
		ComboBox obj5 = comboBox3;					
		ComboBox obj6 = comboBox4;					
		bool flag2 = (((Control)comboBox5).Enabled = Connect);					
		bool flag4 = (((Control)obj6).Enabled = flag2);					
		bool flag6 = (((Control)obj5).Enabled = flag4);					
		bool flag8 = (((Control)obj4).Enabled = flag6);					
		enabled = (((Control)obj3).Enabled = flag8);					
		((Control)obj2).Enabled = enabled;					
	}						
	private void button2_Click(object sender, EventArgs e)						
	{						
		ComboBox obj = comboBox3;					
		int selectedIndex = (((ListControl)comboBox4).SelectedIndex = 10);					
		((ListControl)obj).SelectedIndex = selectedIndex;					
		((ListControl)comboBox5).SelectedIndex = 0;					
	}						
	private void timer1_Tick(object sender, EventArgs e)						
	{						
		//IL_001d: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0027: Expected O, but got Unknown					
		//IL_0045: Unknown result type (might be due to invalid IL or missing references)					
		//IL_004a: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0050: Expected O, but got Unknown					
		//IL_005b: Expected O, but got Unknown					
		//IL_007c: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0081: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0087: Expected O, but got Unknown					
		//IL_009d: Expected O, but got Unknown					
		//IL_00fe: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0108: Unknown result type (might be due to invalid IL or missing references)					
		//IL_011c: Expected O, but got Unknown					
		//IL_011c: Expected O, but got Unknown					
		//IL_013b: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0145: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0159: Expected O, but got Unknown					
		//IL_0159: Expected O, but got Unknown					
		//IL_00be: Unknown result type (might be due to invalid IL or missing references)					
		//IL_00c3: Unknown result type (might be due to invalid IL or missing references)					
		//IL_00c9: Expected O, but got Unknown					
		//IL_00df: Expected O, but got Unknown					
		if (serialPort1.IsOpen && Connect)					
		{					
			bmp = new Bitmap(110, 110);				
			g = Graphics.FromImage((Image)(object)bmp);				
			Graphics obj = g;				
			Pen val = new Pen(Color.Gray, 1f);				
			p = val;				
			obj.DrawEllipse(val, 5, 5, 100, 100);				
			if (Move[0] != 0.0)				
			{				
				Graphics obj2 = g;			
				Pen val2 = new Pen(Color.Red, 3f);			
				p = val2;			
				obj2.DrawLine(val2, 55, 55, 55 + (int)Move[0], 55);			
			}				
			if (Move[1] != 0.0)				
			{				
				Graphics obj3 = g;			
				Pen val3 = new Pen(Color.Green, 3f);			
				p = val3;			
				obj3.DrawLine(val3, 55, 55, 55, 55 - (int)Move[1]);			
			}				
			g.DrawString(Move[0].ToString(), new Font("Arial", 9f), (Brush)new SolidBrush(Color.Red), 0f, 95f);				
			g.DrawString(Move[1].ToString(), new Font("Arial", 9f), (Brush)new SolidBrush(Color.Green), 0f, 0f);				
			pictureBox1.Image = (Image)(object)bmp;				
			p.Dispose();				
			g.Dispose();				
		}					
	}						
	private void timer2_Tick(object sender, EventArgs e)						
	{						
		if (serialPort1.IsOpen && Connect && MouseMode - 1 != ((ListControl)comboBox5).SelectedIndex)					
		{					
			((ListControl)comboBox5).SelectedIndex = MouseMode - 1;				
		}					
	}						
	private static void WriteData(int data1, int data2)						
	{						
		if (serialPort1.IsOpen)					
		{					
			List<byte> list = new List<byte>();				
			list.Add(240);				
			list.Add(252);				
			list.Add((byte)((uint)data1 & 0xFFu));				
			list.Add((byte)((uint)data2 & 0xFFu));				
			list.Add(160);				
			list.Add(160);				
			list.Add(0);				
			list.Add(0);				
			serialPort1.Write(list.ToArray(), 0, list.Count);				
		}					
	}						
	private static void Update()						
	{						
		while (true)					
		{					
			if (serialPort1.IsOpen && Connect)				
			{				
				if (serialPort1.ReadBufferSize <= 8)			
				{			
					continue;		
				}			
				try			
				{			
					if (serialPort1.ReadByte() != 240 || serialPort1.ReadByte() != 252)		
					{		
						continue;	
					}		
					byte[] array = new byte[7];		
					for (byte b = 0; b < 7; b++)		
					{		
						array[b] = Convert.ToByte(serialPort1.ReadByte());	
					}		
					if (array[5] != 160 || array[5] != array[6])		
					{		
						continue;	
					}		
					if (array[0] == 16)		
					{		
						MouseMode = 0;	
						Move[0] = 0.0;	
						Move[1] = 0.0;	
					}		
					else if (array[0] == 17)		
					{		
						MouseMode = 1;	
						int num = array[1] | (array[2] << 8);	
						int num2 = array[3] | (array[4] << 8);	
						if (num >> 15 == 1)	
						{	
							num = -(num & 0x7FFF);
						}	
						if (num2 >> 15 == 1)	
						{	
							num2 = -(num2 & 0x7FFF);
						}	
						Move[0] = (double)num * 0.2;	
						Move[1] = (double)num2 * 0.2;	
						double num3 = Move[0] * Scale[0] * 10.0;	
						double num4 = Move[1] * Scale[1] * 10.0;	
						if (Start)	
						{	
							Cursor.Position = new Point((int)((double)(W / 2) + num3), (int)((double)(H / 2) - num4));
						}	
					}		
					else if (array[0] == 18)		
					{		
						MouseMode = 2;	
						int num5 = array[1] | (array[2] << 8);	
						int num6 = array[3] | (array[4] << 8);	
						if (num5 >> 15 == 1)	
						{	
							num5 = -(num5 & 0x7FFF);
						}	
						if (num6 >> 15 == 1)	
						{	
							num6 = -(num6 & 0x7FFF);
						}	
						Move[0] = (double)num5 * 0.5;	
						Move[1] = (double)num6 * 0.5;	
						if (Start)	
						{	
							Cursor.Position = new Point(Cursor.Position.X + (int)(Move[0] * Scale[0]), Cursor.Position.Y - (int)(Move[1] * Scale[1]));
						}	
					}		
				}			
				catch (Exception)			
				{			
				}			
			}				
			else				
			{				
				Thread.Sleep(1);			
			}				
		}					
	}						
	private void button3_Click(object sender, EventArgs e)						
	{						
		Start = !Start;					
		((Control)button3).Text = (Start ? "종료하기" : "시작하기");					
	}						
	private void comboBox5_SelectedIndexChanged(object sender, EventArgs e)						
	{						
		WriteData(100, ((ListControl)comboBox5).SelectedIndex + 1);					
		WriteData(100, ((ListControl)comboBox5).SelectedIndex + 1);					
	}						
	private void comboBox3_SelectedIndexChanged(object sender, EventArgs e)						
	{						
		Scale[0] = Convert.ToDouble(comboBox3.SelectedItem);					
	}						
	private void comboBox4_SelectedIndexChanged(object sender, EventArgs e)						
	{						
		Scale[1] = Convert.ToDouble(comboBox4.SelectedItem);					
	}						
	private void button4_Click(object sender, EventArgs e)						
	{						
		//IL_000d: Unknown result type (might be due to invalid IL or missing references)					
		MessageBox.Show("반드시 기기를 가만히 놔두고 확인을 누르십시오", "알림", (MessageBoxButtons)0, (MessageBoxIcon)48);					
		Start = false;					
		Move[0] = (Move[1] = 0.0);					
		((Control)button3).Text = (Start ? "종료하기" : "시작하기");					
		WriteData(200, 0);					
		WriteData(200, 0);					
	}						
	protected override void Dispose(bool disposing)						
	{						
		if (disposing && components != null)					
		{					
			components.Dispose();				
		}					
		((Form)this).Dispose(disposing);					
	}						
	private void InitializeComponent()						
	{						
		//IL_000c: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0016: Expected O, but got Unknown					
		//IL_0017: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0021: Expected O, but got Unknown					
		//IL_0022: Unknown result type (might be due to invalid IL or missing references)					
		//IL_002c: Expected O, but got Unknown					
		//IL_002d: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0037: Expected O, but got Unknown					
		//IL_003e: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0048: Expected O, but got Unknown					
		//IL_0049: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0053: Expected O, but got Unknown					
		//IL_0054: Unknown result type (might be due to invalid IL or missing references)					
		//IL_005e: Expected O, but got Unknown					
		//IL_005f: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0069: Expected O, but got Unknown					
		//IL_006a: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0074: Expected O, but got Unknown					
		//IL_0075: Unknown result type (might be due to invalid IL or missing references)					
		//IL_007f: Expected O, but got Unknown					
		//IL_0080: Unknown result type (might be due to invalid IL or missing references)					
		//IL_008a: Expected O, but got Unknown					
		//IL_0091: Unknown result type (might be due to invalid IL or missing references)					
		//IL_009b: Expected O, but got Unknown					
		//IL_08f9: Unknown result type (might be due to invalid IL or missing references)					
		//IL_0903: Expected O, but got Unknown					
		components = new Container();					
		comboBox1 = new ComboBox();					
		comboBox2 = new ComboBox();					
		button1 = new Button();					
		pictureBox1 = new PictureBox();					
		timer1 = new Timer(components);					
		button2 = new Button();					
		button3 = new Button();					
		button4 = new Button();					
		comboBox3 = new ComboBox();					
		comboBox4 = new ComboBox();					
		comboBox5 = new ComboBox();					
		timer2 = new Timer(components);					
		((ISupportInitialize)pictureBox1).BeginInit();					
		((Control)this).SuspendLayout();					
		comboBox1.DropDownStyle = (ComboBoxStyle)2;					
		((ListControl)comboBox1).FormattingEnabled = true;					
		((Control)comboBox1).Location = new Point(93, 14);					
		((Control)comboBox1).Name = "comboBox1";					
		((Control)comboBox1).Size = new Size(121, 20);					
		comboBox1.Sorted = true;					
		((Control)comboBox1).TabIndex = 0;					
		((Control)comboBox1).Click += comboBox1_Click;					
		comboBox2.DropDownStyle = (ComboBoxStyle)2;					
		((ListControl)comboBox2).FormattingEnabled = true;					
		comboBox2.Items.AddRange(new object[12]					
		{					
			300, "600", "1200", "2400", "4800", "9600", "14400", "19200", "28800", "38400",				
			57600, "115200"				
		});					
		((Control)comboBox2).Location = new Point(93, 43);					
		((Control)comboBox2).Name = "comboBox2";					
		((Control)comboBox2).Size = new Size(121, 20);					
		((Control)comboBox2).TabIndex = 1;					
		((Control)button1).Location = new Point(12, 12);					
		((Control)button1).Name = "button1";					
		((Control)button1).Size = new Size(75, 23);					
		((Control)button1).TabIndex = 2;					
		((Control)button1).Text = "연결하기";					
		((ButtonBase)button1).UseVisualStyleBackColor = true;					
		((Control)button1).Click += button1_Click;					
		((Control)pictureBox1).BackColor = Color.Black;					
		((Control)pictureBox1).Location = new Point(220, 12);					
		((Control)pictureBox1).Name = "pictureBox1";					
		((Control)pictureBox1).Size = new Size(110, 110);					
		pictureBox1.TabIndex = 3;					
		pictureBox1.TabStop = false;					
		timer1.Interval = 10;					
		timer1.Tick += timer1_Tick;					
		((Control)button2).Enabled = false;					
		((Control)button2).Location = new Point(12, 41);					
		((Control)button2).Name = "button2";					
		((Control)button2).Size = new Size(75, 23);					
		((Control)button2).TabIndex = 2;					
		((Control)button2).Text = "기본설정";					
		((ButtonBase)button2).UseVisualStyleBackColor = true;					
		((Control)button2).Click += button2_Click;					
		((Control)button3).Enabled = false;					
		((Control)button3).Location = new Point(12, 70);					
		((Control)button3).Name = "button3";					
		((Control)button3).Size = new Size(75, 23);					
		((Control)button3).TabIndex = 2;					
		((Control)button3).Text = "시작하기";					
		((ButtonBase)button3).UseVisualStyleBackColor = true;					
		((Control)button3).Click += button3_Click;					
		((Control)button4).Enabled = false;					
		((Control)button4).Location = new Point(12, 99);					
		((Control)button4).Name = "button4";					
		((Control)button4).Size = new Size(75, 23);					
		((Control)button4).TabIndex = 2;					
		((Control)button4).Text = "센서보정";					
		((ButtonBase)button4).UseVisualStyleBackColor = true;					
		((Control)button4).Click += button4_Click;					
		comboBox3.DropDownStyle = (ComboBoxStyle)2;					
		((Control)comboBox3).Enabled = false;					
		((ListControl)comboBox3).FormattingEnabled = true;					
		comboBox3.Items.AddRange(new object[21]					
		{					
			-5.0, "-4.5", "-4.0", "-3.5", "-3.0", "-2.5", "-2.0", "-1.5", "-1.0", "-0.5",				
			0.0, "+0.5", "+1.0", "+1.5", "+2.0", "+2.5", "+3.0", "+3.5", "+4.0", "+4.5",				
			+5.0				
		});					
		((Control)comboBox3).Location = new Point(93, 72);					
		((Control)comboBox3).Name = "comboBox3";					
		((Control)comboBox3).Size = new Size(58, 20);					
		((Control)comboBox3).TabIndex = 1;					
		comboBox3.SelectedIndexChanged += comboBox3_SelectedIndexChanged;					
		comboBox4.DropDownStyle = (ComboBoxStyle)2;					
		((Control)comboBox4).Enabled = false;					
		((ListControl)comboBox4).FormattingEnabled = true;					
		comboBox4.Items.AddRange(new object[21]					
		{					
			-5.0, "-4.5", "-4.0", "-3.5", "-3.0", "-2.5", "-2.0", "-1.5", "-1.0", "-0.5",				
			0.0, "+0.5", "+1.0", "+1.5", "+2.0", "+2.5", "+3.0", "+3.5", "+4.0", "+4.5",				
			+5.0				
		});					
		((Control)comboBox4).Location = new Point(157, 72);					
		((Control)comboBox4).Name = "comboBox4";					
		((Control)comboBox4).Size = new Size(57, 20);					
		((Control)comboBox4).TabIndex = 1;					
		comboBox4.SelectedIndexChanged += comboBox4_SelectedIndexChanged;					
		comboBox5.DropDownStyle = (ComboBoxStyle)2;					
		((Control)comboBox5).Enabled = false;					
		((ListControl)comboBox5).FormattingEnabled = true;					
		comboBox5.Items.AddRange(new object[2] { "중력가속도 모드", "각가속도 모드" });					
		((Control)comboBox5).Location = new Point(93, 101);					
		((Control)comboBox5).Name = "comboBox5";					
		((Control)comboBox5).Size = new Size(121, 20);					
		((Control)comboBox5).TabIndex = 1;					
		comboBox5.SelectedIndexChanged += comboBox5_SelectedIndexChanged;					
		timer2.Interval = 500;					
		timer2.Tick += timer2_Tick;					
		((ContainerControl)this).AutoScaleDimensions = new SizeF(7f, 12f);					
		((ContainerControl)this).AutoScaleMode = (AutoScaleMode)1;					
		((Form)this).AutoSizeMode = (AutoSizeMode)0;					
		((Form)this).ClientSize = new Size(342, 134);					
		((Control)this).Controls.Add((Control)(object)pictureBox1);					
		((Control)this).Controls.Add((Control)(object)button4);					
		((Control)this).Controls.Add((Control)(object)button3);					
		((Control)this).Controls.Add((Control)(object)button2);					
		((Control)this).Controls.Add((Control)(object)button1);					
		((Control)this).Controls.Add((Control)(object)comboBox4);					
		((Control)this).Controls.Add((Control)(object)comboBox5);					
		((Control)this).Controls.Add((Control)(object)comboBox3);					
		((Control)this).Controls.Add((Control)(object)comboBox2);					
		((Control)this).Controls.Add((Control)(object)comboBox1);					
		((Form)this).MaximizeBox = false;					
		((Control)this).Name = "Form1";					
		((Form)this).StartPosition = (FormStartPosition)1;					
		((Control)this).Text = "AirMouse Application";					
		((Form)this).FormClosing += new FormClosingEventHandler(Form1_FormClosing);					
		((Form)this).Load += Form1_Load;					
		((ISupportInitialize)pictureBox1).EndInit();					
		((Control)this).ResumeLayout(false);					
	}						
