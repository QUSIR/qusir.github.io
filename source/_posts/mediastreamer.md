---
title: mediastreamer使用教程
date: 2016-12-19 21:05:15
tags:
---

#mediastreamer使用教程

##1.各个函数功能简介
`ms_filter_destroy` 释放filter资源

`ms_ticker_destroy`释放ticker

说明：ticker为定时器线程，每隔10ms执行一次

`ms_filter_new`创建filter，传进参数为filter的ID

`ms_connection_helper_start`拿到filter链接起始位置

`ms_connection_helper_link`链接filter

`ms_filter_link`链接filter

`ms_ticker_new`创建ticker

ticker说明：

ticker是属于一个线程不能够运行两个阻塞式的过程，比如socks的发送与接收，必须将
发送和接收放在filter线程里面。

`ms_ticker_set_name`设置ticker名称

`ms_ticker_attach`将ticker附加到线程中

`ms_ticker_detach`去掉ticker

`ms_connection_helper_unlink`去掉filter链接

`ms_filter_unlink` 去掉filter链接

`ms_new filter`数据结构创建

`ms_free` 数据结构数据释放

`ms_queue_get(f->inpputs[0])`获取filter数据

`msgdsize`获取filter里面数据长度

`ms_queue_put(f->outputs[0],om)`往filter里面塞数据

`freemsg`释放filter数据

`ms_thread_join`在filter里面创建线程

`ms_filter_call_method`往filter里面发送数据

##2.创建filter过程

###2.1在Allfilters.h里面添加filter的ID

###2.2在Alldescs.h里面添加filter结构体变量

###2.3在实现filter的源文件里面添加相关头文件

	#include "msfilter.h"
	#include "msticker.h"
###2.4.一个标准的filter过程

	#include "msfilter.h"
	#include "msticker.h"
	
	static void enc_init(MSFilter *f){
	
	}
	
	static void enc_uninit(MSFilter *f){
	
	}
	
	static void enc_preprocess(MSFilter *f)
	{
	}
	
	static void enc_process(MSFilter *f){
	
	
	}
	
	static void enc_postprocess(MSFilter *f){
	}
	
	static MSFilterMethod enc_methods[]={
	        {0,NULL}
	};
	
	MSFilterDesc ms_amr_enc_desc={
	        MS_AMR_ENCODER_ID, //filter ID
	        "amrEnc",  //filter名称
	        "amr encoder",
	        MS_FILTER_ENCODER,  //filter类型MS_FILTER_OTHER 其他,
								//MS_FILTER_ENCODER 编码器,
							    //MS_FILTER_DECODER 解码器
	        "amr",
	        1,
	        1,
	        enc_init,   //初始化操作
	        enc_preprocess,  //预初始化操作
	        enc_process,   //处理过程
	        enc_postprocess,  //预结束操作
	        enc_uninit,  //结束操作
	        enc_methods  //程序模块方法，用于参数传递
	};
	
	
	static void dec_init(MSFilter *f){
	
	}
	
	static void dec_uninit(MSFilter *f){
	
	}
	
	static void dec_preprocess(MSFilter *f){
	}
	
	static void dec_postprocess(MSFilter *f){
	}
	
	static void dec_process(MSFilter *f){
		
	}
	
	static MSFilterMethod dec_methods[]={
	        {0,NULL}
	};
	
	MSFilterDesc ms_amr_dec_desc={
	        MS_AMR_DECODER_ID,
	        "amrDec",
	        "amr decoder",
	        MS_FILTER_DECODER,
	        "amr",
	        1,
	        1,
	        dec_init,
	        dec_preprocess,
	        dec_process,
	        dec_postprocess,
	        dec_uninit,
	        dec_methods
	};
	
	MS_FILTER_DESC_EXPORT(ms_amr_dec_desc)
	MS_FILTER_DESC_EXPORT(ms_amr_enc_desc)


##3.替换编码器

将原来silk编码器换为amr编码器

###3.1创建amr的filter

	stream->decoder=ms_filter_new(MS_AMR_DECODER_ID);


###3.2链接编码filter

	ms_filter_link(stream->tcpRecv,0,stream->decoder,0);


##4.filter创建注意事项

###4.1 filter里面数据流要对应

	MSFilterDesc ms_amr_dec_desc={
	        MS_AMR_DECODER_ID,
	        "amrDec",
	        "amr decoder",
	        MS_FILTER_DECODER,
	        "amr",
	        1,  //进
	        1,  //出
	        dec_init,
	        dec_preprocess,
	        dec_process,
	        dec_postprocess,
	        dec_uninit,
	        dec_methods
	};
以上是编码器filter,是一进一出，原始数据进去出来编码后的数据。

	MSFilterDesc ms_tcpclient_send_desc={
	        MS_TCP_SEND_ID,
	        "TcpClientSend",
	        "TcpClient_Send",
	        MS_FILTER_OTHER,
	        "tcpclient",
	        1,  //进
	        0,  //出
	        tcp_send_init,
			tcp_send_preprocess,
	        tcp_send_process,
			tcp_send_postprocess,
	        tcp_send_uninit,
	        tcpclient_send_methods
	};
以上是TCP数据发送filter，只有进没有出，数据进来之后都发送数据都发送出去了

###4.2 filter里面new的结构体数据要记得free

##5.例子：一个音频流启动过程

	
	#include "mediastreamer/audiostream.h"
	#include "rtpsession.h"
	#include "mediastreamer/msrtp.h"
	#include "mediastreamer/mssndcard.h"
	#include "mediastreamer/msvolume.h"
	#include"mediastreamer/TcpClientFilter.h"
	#include <signal.h>
	#include <stdio.h>
	
	static ms_mutex_t stream_mutex;
	
	//初始化结构体数据
	AudioStream* audio_stream_new() {
		AudioStream *stream = (AudioStream *)ms_new0 (AudioStream, 1);
		return stream;
	}
	//释放音频流
	void audio_stream_free(AudioStream *stream) {
	
		if(stream->source!=NULL)
			ms_filter_destroy(stream->source);
		if(stream->encoder!=NULL)
			ms_filter_destroy(stream->encoder);
	
	        if(stream->tcpSend!=NULL)
	                ms_filter_destroy(stream->tcpSend);
	        if(stream->tcpRecv!=NULL)
	                ms_filter_destroy(stream->tcpRecv);
	        if(stream->dest!=NULL)
	                ms_filter_destroy(stream->dest);
	
	        if(stream->ticker!=NULL)
	        {
	                printf("ms_ticker_destroy begin r 41\n");
			ms_ticker_destroy(stream->ticker);
	                printf("ms_ticker_destroy end  r43 audiostream.c \n");
	        }
	
	
		ms_free(stream);
	        ms_mutex_destroy(&stream_mutex);
	        printf("ms_free(stream) end \n");
	}
	
	#define payload_type_set_number2(pt,n)	(pt)->user_data=(void*)((long)n);
	static void dp_set_payload_type(PayloadType *const_pt, int number, const char *recv_fmtp)
	{
		payload_type_set_number2(const_pt, number);
	
		rtp_profile_set_payload(&av_profile,number,const_pt);
	}
	//启动音频流
	int audio_stream_start(AudioStream *stream,char* SeverIp, int SeverPort, char (*localSessionID)[4], char (*remoteSessioID)[4]){
	
	
	        ms_mutex_init(&stream_mutex,NULL);
	        ms_mutex_lock(&stream_mutex);
		if(stream==NULL)
	        {
	            ms_mutex_unlock(&stream_mutex);
	            return -1;
	        }
	
	        stream->decoder=ms_filter_new(MS_AMR_DECODER_ID);
	        if(stream->decoder==NULL){
	                        return -1;
	        }
	
	        stream->encoder=ms_filter_new(MS_AMR_ENCODER_ID);
	        if(stream->encoder==NULL){
	                        return -2;
	        }
	
	        stream->tcpSend=ms_filter_new(MS_TCP_SEND_ID);
	        if(stream->tcpSend==NULL){
	                ms_mutex_unlock(&stream_mutex);
	                return -9;
	        }
	
	        ms_filter_call_method(stream->tcpSend,MS_TCP_SEND_SET_LOCAL_FRAG,localSessionID[0]);
	        ms_filter_call_method(stream->tcpSend,MS_TCP_SEND_SET_REMOTE_FRAG,remoteSessioID[0]);
	        if(ms_filter_call_method(stream->tcpSend,MS_TCP_SEND_LOGIN,0)!=0){
	                printf("audiostream.c::audio_stream_start_call - TcpSendFilter fail to login\n");
	                ms_mutex_unlock(&stream_mutex);
	                return -19;
	        }
	
	        stream->tcpRecv=ms_filter_new(MS_TCP_READ_ID);
	        if(stream->tcpRecv==NULL){
	                ms_mutex_unlock(&stream_mutex);
	                return -10;
	        }
	        printf("audiostream.c::audio_stream_start_call - local:%02x%02x%02x%02x remote:%02x%02x%02x%02x",localSessionID[1][0]&0xff,localSessionID[1][1]&0xff,localSessionID[1][2]&0xff,localSessionID[1][3]&0xff,
	                  remoteSessioID[1][0]&0xff,remoteSessioID[1][1]&0xff,remoteSessioID[1][2]&0xff,remoteSessioID[1][3]&0xff);
	        ms_filter_call_method(stream->tcpRecv,MS_TCP_READ_SET_LOCAL_FRAG,localSessionID[1]);
	        ms_filter_call_method(stream->tcpRecv,MS_TCP_READ_SET_REMOTE_FRAG,remoteSessioID[1]);
	        if(ms_filter_call_method(stream->tcpRecv,MS_TCP_READ_LOGIN,0)!=0){
	                printf("audiostream.c::audiio_stream_start_call - TcpReadFilter fail to login\n");
	                ms_mutex_unlock(&stream_mutex);
	                return -20;
	        }
	
	        stream->source=ms_filter_new(MS_LINUX_SOUND_READ_ID);
	        if(stream->source==NULL){
	                ms_mutex_unlock(&stream_mutex);
	                return -2;
	        }
	
	        stream->dest=ms_filter_new(MS_LINUX_SOUND_WRITE_ID);
	        if(stream->dest==NULL){
	                ms_mutex_unlock(&stream_mutex);
	                return -8;
	        }
	
	        ms_filter_link(stream->tcpRecv,0,stream->decoder,0);
	        ms_filter_link(stream->decoder,0,stream->dest, 0);
	        ms_filter_link(stream->source,0,stream->encoder,0);
	        ms_filter_link(stream->encoder,0,stream->tcpSend,0);
	
	        stream->ticker = ms_ticker_new();
	        if(stream->ticker==NULL){
	                ms_mutex_unlock(&stream_mutex);
	                return -6;
	        }
	
	        ms_ticker_set_name(stream->ticker,"Audio MSTicker");
	
	        ms_ticker_attach(stream->ticker, stream->source);
	
	        ms_ticker_attach(stream->ticker, stream->tcpRecv);
	
	        ms_mutex_unlock(&stream_mutex);
		return 0;
	}
	//关闭音频流
	void audio_stream_stop(AudioStream *stream) {
			/* detach */
	
	        ms_mutex_lock(&stream_mutex);
	
	        int i=0;
	
	        if(stream->ticker != NULL && stream->source!=NULL)
	                ms_ticker_detach(stream->ticker, stream->source);
	
	        if(stream->ticker != NULL && stream->tcpRecv!=NULL)
	                   ms_ticker_detach(stream->ticker,stream->tcpRecv);
	
	        if(stream->tcpRecv!=NULL && stream->decoder!=NULL)
	                ms_filter_unlink(stream->tcpRecv, 0, stream->decoder, 0);
	
	
	        if(stream->decoder!=NULL && stream->dest!=NULL)
	                ms_filter_unlink(stream->decoder, 0, stream->dest, 0);
	
	        if(stream->source!=NULL && stream->encoder!=NULL)
	                ms_filter_unlink(stream->source, 0, stream->encoder, 0);
	
	        if(stream->encoder!=NULL && stream->tcpSend!=NULL)
	        {
	                printf("ms_filter_unlink(stream->encoder, 0, stream->tcpSend, 0) \n\n");
	                ms_filter_unlink(stream->encoder, 0, stream->tcpSend, 0);
	        }
	
			/* destroy filter */
	        ms_mutex_unlock(&stream_mutex);
			audio_stream_free(stream);
	        printf("audio_stream_stop 10\n");
	}
