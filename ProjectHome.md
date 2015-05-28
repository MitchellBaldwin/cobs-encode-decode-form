# Introduction #
COBS is an algorithm for encoding data bytes into a fixed length frame as opposed to other byte stuffing methods that lead to varying frame lengths. This fixed length of frame make getting the information out of the frame much simpler particularly when using DMA or FIFO buffers.

Full descriptions can be found on Wikipedia:

http://en.wikipedia.org/wiki/Consistent_Overhead_Byte_Stuffing

and Stuart Cheshire and Mary Baker's paper

http://conferences.sigcomm.org/sigcomm/1997/papers/p062.pdf

## Background ##

After having a discussion with my embedded colleges about the framing structure we where using on a UART to PC interface, what a pain it was to take out all the stuffed bytes and how he couldn't DMA it I said "there has got to be a way of producing a fixed length packed".

> After a bit of Googling it turns out there is, COBS is the answer.

## Program ##
> The attached form has two buttons and three text boxes.

> Type a string of hex into the top text box

11220033    (0x11 0x22 0x00 0x33)

and press Encode.

> In the next text box down you will get the COBS encoded value.

0311220233  (0x03 0x11 0x22 0x02 0x33)

then press Decode and in the next box down you will get the decoded value

11220033

## Code ##
```
In "form1.cs" "Form1_Load" there is the following test to show the use of the COBSCOdec.
```
```

   byte[] test = new byte[12];
            byte[] resultEn = new byte[0];
            byte[] resultDc = new byte[0];
            test[0] = 0x45;
            test[1] = 0x00;
            test[2] = 0x00;
            test[3] = 0x2c;
            test[4] = 0x4c;
            test[5] = 0x79;
            test[6] = 0x00;
            test[7] = 0x00;
            test[8] = 0x40;
            test[9] = 0x06;
            test[10] = 0x4f;
            test[11] = 0x37;
            

            resultEn = COBSCodec.encode(test);
            resultDc = COBSCodec.decode(resultEn); 
```
Stick a break point in here and you will the see

4500002C4C79000040064f37

encoded as resultEn

> 024501042c4c79010540064f37

> decoded as  resultDc

4500002C4C79000040064f37

The work horse of the code is COBSCodec.cs this code was adapted from Java code found here.

### History ###
Draft - 14 Mat 2013