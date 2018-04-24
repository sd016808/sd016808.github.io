---
layout: post
title: 'C# 檢查網路磁碟機是否存在'
author: 'James Peng'
tags: ['C#']
---

## 檢查網路磁碟機是否存在 ##

~~~csharp
        /// <summary>
        /// 檢查網路磁碟機是否存在
        /// </summary>
        /// <param name="path">路徑</param>
        /// <param name="timeout">逾時限制 (1s會timeout)</param>
        /// <returns></returns>
        public static bool IsDirectoryExists( string FolderPathReport, string path, int timeout = 2000)
        {
            CancellationTokenSource _cancelTokenSouce;
            CancellationToken _cancelToken;
            _cancelTokenSouce = new CancellationTokenSource();
            _cancelToken = _cancelTokenSouce.Token;

            try
            {
                var result = false;
                Thread tThread = null;
                var t = Task.Factory.StartNew(() =>
                {
                    tThread = Thread.CurrentThread;
                    result = Directory.Exists(FolderPathReport);

                }, _cancelToken, TaskCreationOptions.AttachedToParent, TaskScheduler.Current);
                t.Wait(timeout);
                if (t.IsCompleted == false)
                {
                    tThread.Abort();
                }
                // 取消 Token
                if (_cancelTokenSouce != null)
                    _cancelTokenSouce.Cancel(true);

                return result;
            }
            catch (Exception ex)
            {
                // 取消 Token
                if (_cancelTokenSouce != null)
                    _cancelTokenSouce.Cancel(true);

                //LogManager.WriteLog(ex, false);
                return false;
            }
        }
~~~


