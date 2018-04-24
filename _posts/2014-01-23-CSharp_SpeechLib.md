---
layout: post
title: 'C# 語音辨識與文字轉語音應用 Text-to-Speech '
author: 'James Peng'
tags: ['C# 3rd-Party Library']
---

在COM元件中選擇Microsoft Speech Object Library 5.4

![](..\images\2014-01-23-CSharp_SpeechLib\Q8LXnfd.png)


----------

## 加入命名空間 ##

~~~csharp
using SpeechLib;
~~~

----------

## 文字轉語音 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            SpVoiceClass voice = new SpVoiceClass();
            voice.Voice = voice.GetVoices(string.Empty, string.Empty).Item(0);
            voice.Speak("Welcome to text to speech system.", SpeechVoiceSpeakFlags.SVSFDefault);//SVSFDefault: Specifies that the default settings

            System.Threading.Thread.Sleep(3000);
            voice.Speak("Warning, SPC chart is out of upper control limit!", SpeechVoiceSpeakFlags.SVSFDefault);

            System.Threading.Thread.Sleep(3000);
            voice.Speak("Alarm, SPC chart is out of upper spec limit!", SpeechVoiceSpeakFlags.SVSFDefault);
        }
~~~


----------
## 語音轉文字 ##

~~~csharp
        private void button1_Click(object sender, EventArgs e)
        {
            SpSharedRecoContextClass objRecoContext = null; ;
            ISpeechRecoGrammar grammar;
            objRecoContext = new SpSharedRecoContextClass();
            objRecoContext.Recognition += new _ISpeechRecoContextEvents_RecognitionEventHandler(objRecoContext_Recognition);
            grammar = objRecoContext.CreateGrammar(0);
            grammar.DictationLoad("", SpeechLoadOption.SLOStatic);
            grammar.DictationSetState(SpeechRuleState.SGDSActive);
        }

		public void objRecoContext_Recognition(int StreamNumber, object StreamPosition, SpeechRecognitionType RecognitionType, ISpeechRecoResult Result)
		{
		    string recgData = Result.PhraseInfo.GetText(0, -1, true);
		    textBox1.Text = recgData;
		    Speak(recgData);
		    recgData = "";
		}
~~~

![](..\images\2014-01-23-CSharp_SpeechLib\ojcYApA.png)

----------

參考：

- [http://msdn.microsoft.com/en-us/library/ee125663(v=vs.85).aspx](http://msdn.microsoft.com/en-us/library/ee125663(v=vs.85).aspx)
- http://einboch.pixnet.net/blog/post/267347720
- http://einboch.pixnet.net/blog/post/267419177
