# DTcms
动力启航网站管理系统（简称DTcms），是目前国内ASP.NET开源界少见的优秀开源管理系统，基于 ASP.NET(C#)+ MSSQL(ACCESS) 的技术开发，全部100%免费开放源代码。

## DES密钥加密解密
`SELECT TOP 20 * from [dt_manager] ORDER BY 1 DESC;`

https://www.programiz.com/csharp-programming/online-compiler/

```c#
// Online C# Editor for free
// Write, Edit and Run your C# code using C# Online Compiler

using System;
using System.Security.Cryptography;
using System.Text;
public class DESEncrypt
{
    #region ========加密========

    /// <summary>
    /// 加密
    /// </summary>
    /// <param name="Text"></param>
    /// <returns></returns>
    public static string Encrypt(string Text)
    {
        return Encrypt(Text, "DTcms");
    }
    /// <summary>
    /// 加密数据
    /// </summary>
    /// <param name="Text"></param>
    /// <param name="sKey"></param>
    /// <returns></returns>
    public static string Encrypt(string Text, string sKey)
    {
        DESCryptoServiceProvider des = new DESCryptoServiceProvider();
        byte[] inputByteArray;
        inputByteArray = Encoding.Default.GetBytes(Text);
        des.Key = ASCIIEncoding.ASCII.GetBytes(System.Web.Security.FormsAuthentication.HashPasswordForStoringInConfigFile(sKey, "md5").Substring(0, 8));
        des.IV = ASCIIEncoding.ASCII.GetBytes(System.Web.Security.FormsAuthentication.HashPasswordForStoringInConfigFile(sKey, "md5").Substring(0, 8));
        System.IO.MemoryStream ms = new System.IO.MemoryStream();
        CryptoStream cs = new CryptoStream(ms, des.CreateEncryptor(), CryptoStreamMode.Write);
        cs.Write(inputByteArray, 0, inputByteArray.Length);
        cs.FlushFinalBlock();
        StringBuilder ret = new StringBuilder();
        foreach (byte b in ms.ToArray())
        {
            ret.AppendFormat("{0:X2}", b);
        }
        return ret.ToString();
    }

    #endregion

    #region ========解密========

    /// <summary>
    /// 解密
    /// </summary>
    /// <param name="Text"></param>
    /// <returns></returns>
    public static string Decrypt(string Text)
    {
        return Decrypt(Text, "DTcms");
    }
    /// <summary>
    /// 解密数据
    /// </summary>
    /// <param name="Text"></param>
    /// <param name="sKey"></param>
    /// <returns></returns>
    public static string Decrypt(string Text, string sKey)
    {
        DESCryptoServiceProvider des = new DESCryptoServiceProvider();
        int len;
        len = Text.Length / 2;
        byte[] inputByteArray = new byte[len];
        int x, i;
        for (x = 0; x < len; x++)
        {
            i = Convert.ToInt32(Text.Substring(x * 2, 2), 16);
            inputByteArray[x] = (byte)i;
        }
        des.Key = ASCIIEncoding.ASCII.GetBytes(System.Web.Security.FormsAuthentication.HashPasswordForStoringInConfigFile(sKey, "md5").Substring(0, 8));
        des.IV = ASCIIEncoding.ASCII.GetBytes(System.Web.Security.FormsAuthentication.HashPasswordForStoringInConfigFile(sKey, "md5").Substring(0, 8));
        System.IO.MemoryStream ms = new System.IO.MemoryStream();
        CryptoStream cs = new CryptoStream(ms, des.CreateDecryptor(), CryptoStreamMode.Write);
        cs.Write(inputByteArray, 0, inputByteArray.Length);
        cs.FlushFinalBlock();
        return Encoding.Default.GetString(ms.ToArray());
    }

    #endregion


    public static void Main(string[] args)
    {
        Console.WriteLine ("Hello Mono World");
        Console.WriteLine (Decrypt("68564933608EC78C","8XZL88"));
        Console.WriteLine (Decrypt("6F0936315BA3646A","4R04B6"));
    }
}
```

![image](https://user-images.githubusercontent.com/16593068/218359901-4c8e80a3-c220-451c-987b-9a9e13128fb5.png)
