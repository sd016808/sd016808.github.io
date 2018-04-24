---
layout: post
title: 'C# 取得 CRC8 CRC16 CRC32'
author: 'James Peng'
tags: ['C#']
---


## CheckSum.cs ##

~~~csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace DPST150DBAGUI
{
    /// <summary>
    /// http://www.lammertbies.nl/comm/info/crc-calculation.html
    /// </summary>
    public class CheckSum
    {
      
        public enum Protocol
        {
            /// <summary>
            /// Each bus transaction requires a Packet Error Code (PEC) calculation by both the transmitter and receiver
            /// within each packet. The PEC uses an 8-bit cyclic redundancy check (CRC-8) of each read or write bus
            /// transaction to calculate a Packet Error Code (PEC). The PEC may be calculated in any way that conforms
            /// to a CRC-8 represented by the polynomial, C(x) = x8 + x2 + x1 + 1 and must be calculated in the order of
            /// the bits as received.
            /// </summary>
            SMBus_Normal,
            /// <summary>
            /// Packet Error Checking. See the SMBus specification, Version 2.0 [A03] for more information.
            /// </summary>
            SMBus_Reversed,
            /// <summary>
            /// CRC-16-IBM
            /// </summary>
            ModBus_Normal,
            ModBus_Reversed,
        }

        public static string GetCrc(Protocol protocal ,string strInput)
        {
            switch (protocal) {
                case Protocol.SMBus_Normal:
                    Crc8.SetPoly(Crc8.Crc8Type.CRC8_CCITT , Crc8.Representations.Normal);
                    return GetCrc8(strInput);
                case Protocol.SMBus_Reversed:
                    Crc8.SetPoly(Crc8.Crc8Type.CRC8_CCITT , Crc8.Representations.Reversed);
                    return GetCrc8(strInput);
                case Protocol.ModBus_Normal:
                    Crc16.SetPoly(Crc16.Crc16Type.CRC16_IBM, Crc16.Representations.Normal);
                    return GetCrc16(strInput);
                case Protocol.ModBus_Reversed:
                     Crc16.SetPoly(Crc16.Crc16Type.CRC16_IBM, Crc16.Representations.Reversed);
                     return GetCrc16(strInput);
                default :
                     Crc8.SetPoly(Crc8.Crc8Type.CRC8_CCITT, Crc8.Representations.Normal);
                    return GetCrc8(strInput);   
            }
        }

        public static string GetCrc8(string strInput)
        {
            strInput = strInput.Trim();
            string[] strInputs = strInput.Split(' ');
            byte[] byteInput = new byte[strInputs.Length];
            for (int i = 0; i < strInputs.Length; i++)
            {
                byteInput[i] = Convert.ToByte(strInputs[i],16);
            }
            byte crc = Crc8.ComputeChecksum(byteInput);
            return crc.ToString("X2").ToUpper().Trim();
        }

        public static string GetCrc16(string strInput)
        {
            strInput = strInput.Trim();
            string[] strInputs = strInput.Split(' ');
            byte[] byteInput = new byte[strInputs.Length];
            for (int i = 0; i < strInputs.Length; i++)
            {
                byteInput[i] = Convert.ToByte(strInputs[i], 16);
            }
            byte[] crc = Crc16.ComputeChecksumBytes(byteInput);
            return BitConverter.ToString(crc).Replace("-"," ").Trim();
        }
    }





    /// <summary>
    /// http://sanity-free.org/146/crc8_implementation_in_csharp.html
    /// byte crc = Crc8.ComputeChecksum( 1, 2, 3 );
    /// byte check = Crc8.ComputeChecksum( 1, 2, 3, crc );
    /// // here check should equal 0 to show that the checksum is accurate
    /// if ( check != 0 ) {
    ///     Console.WriteLine( "Error in the checksum" );
    /// }
    /// </summary>
    public static class Crc8
    {
        public enum Representations
        {
            Normal,
            Reversed ,
            ReverseOfReciprocal
        }

        public enum Crc8Type
        {
            /// <summary>
            /// x8 + x2 + x1 + 1
            /// </summary>
            CRC8_CCITT,
            /// <summary>
            /// x8 + x5 + x4 + 1
            /// </summary>
            CRC8_DallasMaxim,
            /// <summary>
            /// x8 + x7 + x6 + x4 + x2 + 1
            /// </summary>
            CRC8,
            /// <summary>
            /// x8 + x4 + x3 + x2 + 1
            /// </summary>
            CRC8_SAEJ1850,
            /// <summary>
            /// x8 + x7 + x4 + x3 + x1 + 1
            /// </summary>
            CRC8_WCDMA
        }

        static byte[] table = new byte[256];
        
        private static byte poly = 0x07;//0xd5;

        public static void SetPoly(Crc8Type type ,Representations rep)
        {
            switch (type)
            {
                case Crc8Type.CRC8_CCITT:
                    // x8 + x2 + x + 1
                    switch (rep)
                    { 
                        case Representations.Normal:
                            poly = 0x07;
                            break;
                        case Representations.Reversed:
                            poly = 0xE0;
                            break;
                        case Representations.ReverseOfReciprocal:
                            poly = 0x83;
                            break;
                    }
                    break;
                case Crc8Type.CRC8_DallasMaxim:
                    // x8 + x5 + x4 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            poly = 0x31;
                            break;
                        case Representations.Reversed:
                            poly = 0x8C;
                            break;
                        case Representations.ReverseOfReciprocal:
                            poly = 0x98;
                            break;
                    }
                    break;
                case Crc8Type.CRC8:
                    // x8 + x7 + x6 + x4 + x2 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            poly = 0xD5;
                            break;
                        case Representations.Reversed:
                            poly = 0xAB;
                            break;
                        case Representations.ReverseOfReciprocal:
                            poly = 0xEA;
                            break;
                    }
                    break;
                case Crc8Type.CRC8_SAEJ1850:
                    // x8 + x4 + x3 + x2 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            poly = 0x1D;
                            break;
                        case Representations.Reversed:
                            poly = 0xB8;
                            break;
                        case Representations.ReverseOfReciprocal:
                            poly = 0x8E;
                            break;
                    }
                    break;
                case Crc8Type.CRC8_WCDMA:
                    // x8 + x7 + x4 + x3 + x + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            poly = 0x9B;
                            break;
                        case Representations.Reversed:
                            poly = 0xD9;
                            break;
                        case Representations.ReverseOfReciprocal:
                            poly = 0xCD;
                            break;
                    }
                    break;
            }
            
        }

        public static byte ComputeChecksum(params byte[] bytes)
        {
            UpdateTableCrc8();

            byte crc = 0;
            if (bytes != null && bytes.Length > 0)
            {
                foreach (byte b in bytes)
                {
                    crc = table[crc ^ b];
                }
            }
            return crc;
        }

        private static void UpdateTableCrc8()
        {
            for (int i = 0; i < 256; ++i)
            {
                int temp = i;
                for (int j = 0; j < 8; ++j)
                {
                    if ((temp & 0x80) != 0)
                    {
                        temp = (temp << 1) ^ poly;
                    }
                    else
                    {
                        temp <<= 1;
                    }
                }
                table[i] = (byte)temp;
            }
        }
    }




    /// <summary>
    /// http://sanity-free.org/134/standard_crc_16_in_csharp.html
    /// </summary>
    public class Crc16
    {
        public enum Representations
        {
            Normal,
            Reversed,
            ReverseOfReciprocal
        }

        public enum Crc16Type
        {
            /// <summary>
            /// x16 + x15 + x2 + 1
            /// </summary>
            CRC16_IBM,
            /// <summary>
            /// x16 + x12 + x5 + 1
            /// </summary>
            CRC16_CCITT,     
            /// <summary>
            /// x16 + x12 + x11 + x9 + x8 + x7 + x5 + x4 + x2 + x1 + 1
            /// </summary>
            CRC16_T10_DIF,
            /// <summary>
            /// x16 + x12 + x11 + x9 + x8 + x7 + x5 + x4 + x2 + x1 + 1
            /// </summary>
            CRC16_DNP,
            /// <summary>
            /// x16 + x10 + x8 + x7 + x3 + 1
            /// </summary>
            CRC16_DECT            
        }

        private static ushort polynomial = 0xA001; //0x8005

        /// <summary>
        /// https://en.wikipedia.org/wiki/Polynomial_representations_of_cyclic_redundancy_checks
        /// </summary>
        /// <param name="type"></param>
        /// <param name="rep"></param>
        public static void SetPoly(Crc16Type type, Representations rep)
        {
            switch (type)
            {
                case Crc16Type.CRC16_IBM:
                    // x16 + x15 + x2 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x8005;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xA001;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0xC002;
                            break;
                    }
                    break;
                case Crc16Type.CRC16_CCITT:
                    // x16 + x12 + x5 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x1021;
                            break;
                        case Representations.Reversed:
                            polynomial = 0x8408;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0x8810;
                            break;
                    }
                    break;
                case Crc16Type.CRC16_T10_DIF:
                    // x16 + x12 + x11 + x9 + x8 + x7 + x5 + x4 + x2 + x1 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x8BB7;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xEDD1;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0xC5DB;
                            break;
                    }
                    break;
                case Crc16Type.CRC16_DNP:
                    // x16 + x13 + x12 + x11 + x10 + x8 + x6 + x5 + x2 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x3D65;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xA6BC;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0x9EB2;
                            break;
                    }
                    break;
                case Crc16Type.CRC16_DECT:
                    // x16 + x10 + x8 + x7 + x3 + 1
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x0589;
                            break;
                        case Representations.Reversed:
                            polynomial = 0x91A0;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0x82C4;
                            break;
                    }
                    break;
            }

        }


        private static ushort[] table = new ushort[256];

        public static ushort ComputeChecksum(byte[] bytes)
        {
            UpdateTableCrc16();
            ushort crc = 0;
            for (int i = 0; i < bytes.Length; ++i)
            {
                byte index = (byte)(crc ^ bytes[i]);
                crc = (ushort)((crc >> 8) ^ table[index]);
            }
            return crc;
        }

        public static byte[] ComputeChecksumBytes(byte[] bytes)
        {            
            ushort crc = ComputeChecksum(bytes);
            return BitConverter.GetBytes(crc);
        }

        private static void UpdateTableCrc16()
        {
            ushort value;
            ushort temp;
            for (ushort i = 0; i < table.Length; ++i)
            {
                value = 0;
                temp = i;
                for (byte j = 0; j < 8; ++j)
                {
                    if (((value ^ temp) & 0x0001) != 0)
                    {
                        value = (ushort)((value >> 1) ^ polynomial);
                    }
                    else
                    {
                        value >>= 1;
                    }
                    temp >>= 1;
                }
                table[i] = value;
            }
        }
    }



    /// <summary>
    /// http://sanity-free.org/133/crc_16_ccitt_in_csharp.html
    /// </summary>
    public enum InitialCrcValue { Zeros, NonZero1 = 0xffff, NonZero2 = 0x1D0F }

    public class Crc16Ccitt
    {
        public enum Representations
        {
            Normal,
            Reversed,
            ReverseOfReciprocal
        }

        const ushort poly = 0x1021;//4129;
        ushort[] table = new ushort[256];
        ushort initialValue = 0;

        public ushort ComputeChecksum(byte[] bytes)
        {
            ushort crc = this.initialValue;
            for (int i = 0; i < bytes.Length; ++i)
            {
                crc = (ushort)((crc << 8) ^ table[((crc >> 8) ^ (0xff & bytes[i]))]);
            }
            return crc;
        }

        public byte[] ComputeChecksumBytes(byte[] bytes)
        {
            ushort crc = ComputeChecksum(bytes);
            return BitConverter.GetBytes(crc);
        }

        public Crc16Ccitt(InitialCrcValue initialValue)
        {
            this.initialValue = (ushort)initialValue;
            ushort temp, a;
            for (int i = 0; i < table.Length; ++i)
            {
                temp = 0;
                a = (ushort)(i << 8);
                for (int j = 0; j < 8; ++j)
                {
                    if (((temp ^ a) & 0x8000) != 0)
                    {
                        temp = (ushort)((temp << 1) ^ poly);
                    }
                    else
                    {
                        temp <<= 1;
                    }
                    a <<= 1;
                }
                table[i] = temp;
            }
        }
    }



    /// <summary>
    /// http://sanity-free.org/12/crc32_implementation_in_csharp.html
    /// </summary>
    public class Crc32
    {
        public enum Representations
        {
            Normal,
            Reversed,
            ReverseOfReciprocal
        }

        public enum Crc32Type
        {
            CRC32,
            CRC32_C,
            CRC32_K,
            CRC32_Q            
        }

        uint[] table;
        private ulong polynomial = 0x04C11DB7;

        public void SetPoly(Crc32Type type, Representations rep)
        {
            switch (type)
            {
                case Crc32Type.CRC32:                    
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x04C11DB7;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xEDB88320;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0x82608EDB;
                            break;
                    }
                    break;
                case Crc32Type.CRC32_C:                    
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x1EDC6F41;
                            break;
                        case Representations.Reversed:
                            polynomial = 0x82F63B78;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0x8F6E37A0;
                            break;
                    }
                    break;
                case Crc32Type.CRC32_K:                    
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x741B8CD7;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xEB31D82E;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0xBA0DC66B;
                            break;
                    }
                    break;
                case Crc32Type.CRC32_Q:                    
                    switch (rep)
                    {
                        case Representations.Normal:
                            polynomial = 0x814141AB;
                            break;
                        case Representations.Reversed:
                            polynomial = 0xD5828281;
                            break;
                        case Representations.ReverseOfReciprocal:
                            polynomial = 0xC0A0A0D5;
                            break;
                    }
                    break;
              
            }

        }


        public void SetPoly(byte p)
        {
            polynomial = p;
        }

        public uint ComputeChecksum(byte[] bytes)
        {
            uint crc = 0xffffffff;
            for (int i = 0; i < bytes.Length; ++i)
            {
                byte index = (byte)(((crc) & 0xff) ^ bytes[i]);
                crc = (uint)((crc >> 8) ^ table[index]);
            }
            return ~crc;
        }

        public byte[] ComputeChecksumBytes(byte[] bytes)
        {
            return BitConverter.GetBytes(ComputeChecksum(bytes));
        }

        public Crc32()
        {
            
            table = new uint[256];
            uint temp = 0;
            for (uint i = 0; i < table.Length; ++i)
            {
                temp = i;
                for (int j = 8; j > 0; --j)
                {
                    if ((temp & 1) == 1)
                    {
                        temp = (uint)((temp >> 1) ^ polynomial);
                    }
                    else
                    {
                        temp >>= 1;
                    }
                }
                table[i] = temp;
            }
        }
    }


}


~~~


