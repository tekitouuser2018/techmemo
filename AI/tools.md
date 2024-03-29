### AUTO-GPT

https://github.com/Torantulino/Auto-GPT

https://twitter.com/AlphaSignalAI/status/1645847165066006529

#### メモリ関連

- abc抽象基底クラスの使用: 
https://docs.python.org/ja/3/library/abc.html

- メモリクラスをシングルトンで取り扱い: 
https://github.com/Torantulino/Auto-GPT/blob/master/scripts/memory/base.py

- メモリはpinecone or redis or weaviate or milvusを使用し、どちらも存在しなければローカルファイルを使用する: 
https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/memory/__init__.py#L37-L78


- pinecone: 
https://www.pinecone.io

https://dev.classmethod.jp/articles/dive-deep-into-modern-data-saas-about-pinecone/

#### 読み上げ関連

- 読み上げ機能は、elevenlabsのAPIかmacOSの機能かBrianSpeechかpythonのplaysoundを使用している
https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/speech/say.py

#### OpenAI API
openaiのAPIの使用は２種類

openai.ChatCompletion.create

https://github.com/Significant-Gravitas/Auto-GPT/blob/master/autogpt/llm_utils.py#L81-L96

openai.Embedding.create

https://github.com/Significant-Gravitas/Auto-GPT/blob/master/autogpt/llm_utils.py#L128-L138

* execute_command

  * brows_website

    https://github.com/Significant-Gravitas/Auto-GPT/blob/master/autogpt/app.py#L163-L164

    https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/commands/web_selenium.py#L24-L43

    * seleniumとbeautifulsoupを使用してスクレイピング

      https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/commands/web_selenium.py#L46-L97
      
    * テキストをchunkごとに分割してChatpGPTに投げて要約し、それらを連結したものをさらに要約

      https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/processing/text.py#L42-L99
      
      https://github.com/Significant-Gravitas/Auto-GPT/blob/ad7cefa10c0647feee85114d58559fcf83ba6743/autogpt/llm_utils.py#L53

----
### babyagi

https://github.com/yoheinakajima/babyagi

----
