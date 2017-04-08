using System;
using System.Windows;
using System.Windows.Controls;
using System.Net;
using System.Threading;
using Newtonsoft.Json.Linq;
using System.ComponentModel;

namespace WpfApplication1
{
    /// <summary>
    /// Interaction logic for MainWindow.xaml
    /// </summary>
    /// Below is the code for the application assignment for Zypher Sleep Technologies
    /// Completed by James Gilders 
    public partial class MainWindow : Window
    {
        Boolean ThreadContinue = false;
        delegate void TextBoxDel(int value);
        Thread OperationsThread;
        public MainWindow()
        {
            InitializeComponent();
        }

        //Listener for the button controls that starts the new thread
        private void button_Click(object sender, RoutedEventArgs e)
        {
           Button B = (Button) sender;
            String Content = (String) B.Content;
            if (Content == "Start")
            {
                B.Content = "Stop";
                ThreadContinue = true;
                OperationsThread = new Thread(new ThreadStart(Operations));
                OperationsThread.Start();
            }
            else
            {
                B.Content = "Start";
                ThreadContinue = false;
            }
        }
        //The operations thread will be initiated here. This is where the work for the thread will be done
        private void Operations()
        {
            int Calculation;
            while (ThreadContinue == true)
            {
               Calculation = (Calculate(GrabData()));
                listBox.Dispatcher.Invoke(new TextBoxDel(MoveToDisplay), new object[] { Calculation });
                Thread.Sleep(1000);
            }
        }
        
        //Fetches the Json data from the website and returns it as an array with the parameters and the opperation
        private object [] GrabData()
        {
            object[] output = new object[3];
            string url = @"http://api.zephyrportal.com/test.ashx";
            String json = new WebClient().DownloadString(url);
            JObject o = JObject.Parse(json);
            output[0] = (int)o["parm1"];
            output[1] = (int)o["parm2"];
            output[2] = (String)o["op"];
            return output;
        }
        
        //Takes in an array of three strings and then performs the correct action on them
        private int Calculate(object [] parameters)
        {
            int P1 = (int)parameters[0];
            int P2 = (int) parameters[1];
            String P3 = (String) parameters[2];
            if(P3 == "*")
            {
                return (P1 * P2);
            }else if(P3 == "+")
            {
                return (P1 + P2);
            }else if(P3 == "-")
            {
                return (P1 - P2);
            }else
            {
                return (P1 / P2);
            }     
        }

    

        //Simply updates the display with a new value
        private void MoveToDisplay(int output)
        {
                listBox.Items.Add(output);
        }

        private void textBox_TextChanged(object sender, TextChangedEventArgs e)
        {
        }

        private void listBox_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
        }
    }
}
