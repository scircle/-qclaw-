# -qclaw-
在微信ClawBot中，一句话自动生成适合中小学生晨读的诗词word版

# 使用方法
比如在腾讯的OpenClaw(龙虾)环境，概要下的免密登录，进入linux (Ubuntu 24)后（此时是root），
1) cd /root/.openclaw/workspace/skills
2) mkdir -p poem_word_assistant
3) cd poem_word_assistant
4) 把此github上poem_word_assistant目录下几个文件下载，放到/root/.openclaw/workspace/skills/poem_word_assistant
5) 运行此命令openclaw gateway restart
6) 在微信(WeChat)的ClawBot中输入如下关键词" 生成诗词word: <诗词名1>, <诗词名2> [可加作者]
   例子：
   生成诗词word: 卖炭翁， 望岳（杜甫版）

   备注：
   a) 最小要求是2首
   
   b) 关键词可以是
   - 生成诗词word
   - 生成古诗word
   - 诗词word
   - 古诗word
   - 唐诗词word
   - 把某几首诗生成word
   - 根据诗名生成word
     一般不会和qclaw自带的tencent-docs这个skill 冲突
7）即可得到一份带拼音的word版诗词
   

# 可能需要的python包
- pip install pypinyin
- pip install python-docx

# 生成诗词的字体字号简要
   第一行是固定文字 (28号，华文琥珀）
   诗词名、朝代和作者是20号（楷体）
   中文是16号楷体
   每个汉字上拼音是12号楷体
   
