### AUTO-GPT

https://github.com/Torantulino/Auto-GPT

https://twitter.com/AlphaSignalAI/status/1645847165066006529

#### メモリ関連

- abc抽象基底クラスの使用: 
https://docs.python.org/ja/3/library/abc.html

- メモリクラスをシングルトンで取り扱い: 
https://github.com/Torantulino/Auto-GPT/blob/master/scripts/memory/base.py

- メモリはpineconeまたはredisを使用し、どちらも存在しなければローカルファイルを使用する: 
https://github.com/Torantulino/Auto-GPT/blob/98efd264560983ed1d383222e3d5d22ed87169be/scripts/memory/__init__.py#L23..L46

https://github.com/Torantulino/Auto-GPT/blob/master/scripts/memory/local.py#L25..L45

- pinecone: 
https://www.pinecone.io

https://dev.classmethod.jp/articles/dive-deep-into-modern-data-saas-about-pinecone/

#### 読み上げ関連

- 読み上げ機能は、elevenlabsのAPIかmacOSの機能かpythonのplaysoundを使用している
https://github.com/Torantulino/Auto-GPT/blob/master/scripts/speak.py#L74..L91

#### OpenAI API
openaiのAPIの使用は２種類

openai.ChatCompletion.create

https://github.com/Significant-Gravitas/Auto-GPT/blob/master/autogpt/llm_utils.py#L81-L96

openai.Embedding.create

https://github.com/Significant-Gravitas/Auto-GPT/blob/master/autogpt/llm_utils.py#L128-L138

----
### babyagi

https://github.com/yoheinakajima/babyagi

----
