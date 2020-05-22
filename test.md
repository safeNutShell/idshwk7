一、编码加密过程
  本次数据隐写采用了LSB+AES-128-ECB的方式。原始图片为orig.png，要隐写的数据存储在info.txt中，其中存储的密文，相应的AES密钥存储在key.txt中。
  1、首先利用python工具LSBSteg.py将info.txt的信息写入orig.png中，生成tmp.png文件。
     python LSBSteg.py encode -i orig.png -o tmp.png -f info.txt
  2、将存有密钥信息的key.txt通过cat命令添加到tmp.png的尾部，形成最终的test.png。
     cat tmp.png key.txt > test.png
     
二、解码过程
  1、首先利用strings命令查看test.png，在最后一行得到有用信息————"SEU" gonna be useful.
  2、利用python工具LSBSteg.py对test.png进行解码，得到文件cipher.txt，其中保存了数据信息的密文。
     python LSBSteg.py decode -i test.png -o cipher.txt
     注：此时对test.png运行strings命令仍可看见必要信息，即上述两步不会因为执行顺序问题导致信息受损。
  3、将得到的密文和密钥用AES-128-ECB(zero padding)进行解密，得到最终的信息————"57117215WangShaoKang".
  
备注：
  1、信息中姓名采用拼音是担心部分AES实现不支持中文字符。此次实验使用的在线AES加解工具为https://the-x.cn/cryptography/Aes.aspx，
     模式为UTF-8,ECB,Zero,128bits,密钥为SEU
  2、编码过程中必须先用LSBSteg.py编辑LSB信息，再用cat添加txt文件，如果顺序颠倒会导致原本的txt信息丢失。猜测是py程序算法实现所致。但解码过程的1、2步
     顺序随意。
  3、此方法的意图在于实践所学的信息隐藏技术，并做一个结合。但是AES本身有多种模式可选，具体算法实现也各有千秋，即使有用密文和密钥也不一定能得到正确明文，
     因此明确给出工具和参数。此处尚可改进。
