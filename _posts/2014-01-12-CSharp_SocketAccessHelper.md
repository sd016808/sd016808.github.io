---
layout: post
title: 'C# 透過 Socket Send 資料'
author: 'James Peng'
tags: ['C#']
---


## SocketAccessHelper ##

~~~csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Net;
using System.Net.Sockets;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace AccessComponents
{
    public class SocketAccessHelper
    {        
        private static TcpClient client = null;
        private static IPEndPoint localEndPoint = null;      
        private static byte[] buffer = new byte[4096];

        public static bool close()
        {
            if (client != null)
            {
                client.Close();
                return true;
            }
            return false;                       
        }

        public static bool connect(string strIP, int iPort)
        {            
            client = new TcpClient();
            Socket sck;
            localEndPoint = new IPEndPoint(IPAddress.Parse(strIP), iPort);
            sck = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            try
            {
                client.Connect(localEndPoint);
                Console.ForegroundColor = ConsoleColor.Green;
                Console.WriteLine("Server is online: {0}", localEndPoint.ToString());
                Console.ResetColor();
                return true;
            }
            catch
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("Server is offline: {0} \nTrying to reconnect...", localEndPoint.ToString());
                Console.ResetColor();
                return false;
            }            
        }


        public static bool Send(string strData)
        {

            byte[] tx;
            if (string.IsNullOrEmpty(strData))
                return false;

            try
            {
                tx = Encoding.ASCII.GetBytes(strData);

                if (client != null)
                {
                    if (client.Client.Connected)
                    {
                        client.GetStream().BeginWrite(tx, 0, tx.Length, onCompleteWriteServer, client);
                    }
                }

                return true;
            }

            catch (Exception exc)
            {
                MessageBox.Show(exc.Message);
                return false;
            }
        }

        private static void onCompleteWriteServer(IAsyncResult iar)
        {
            TcpClient tcpc;

            try
            {
                tcpc = (TcpClient)iar.AsyncState;
                tcpc.GetStream().EndWrite(iar);
            }
            catch (Exception exc)
            {
                MessageBox.Show(exc.Message);

            }

        }

     

        public static string WriteByTcpClient(string strIP,int iPort, string strWriteText)
        {
            TcpClient tcp = new TcpClient(strIP, iPort); 
            NetworkStream stream = new NetworkStream(tcp.Client);
            StreamReader sr = new StreamReader(stream);
            StreamWriter sw = new StreamWriter(stream);

            try
            {
                sw.WriteLine(strWriteText);
                sw.Flush();
            }
            catch
            {
                return "";
            }
       
            return sr.ReadLine();        
        }



        public static string WriteBySocket(string strIP, int iPort, string strWriteText)
        {

            
            Socket socket = new Socket(AddressFamily.InterNetwork, SocketType.Stream, ProtocolType.Tcp);
            socket.Connect(strIP, iPort);            
            NetworkStream stream = new NetworkStream(socket);
            StreamReader sr = new StreamReader(stream);
            StreamWriter sw = new StreamWriter(stream);
            string output = "";
            try
            {
                sw.WriteLine(strWriteText);
                sw.Flush();
                output = sr.ReadLine();
            }
            catch
            {
                return "";
            }
            sw.Close();
            sr.Close();
            stream.Close();
           
            return output;
            
        }


        /// <summary>
        /// 取得本機 IP Address
        /// </summary>
        /// <returns></returns>
        public static List<string> GetHostIPAddress()
        {
            List<string> lstIPAddress = new List<string>();
            IPHostEntry IpEntry = Dns.GetHostEntry(Dns.GetHostName());
            foreach (IPAddress ipa in IpEntry.AddressList)
            {
                if (ipa.AddressFamily == AddressFamily.InterNetwork)
                    lstIPAddress.Add(ipa.ToString());
            }
            return lstIPAddress; // result: 192.168.1.17 ......
        }

    }
}

~~~


