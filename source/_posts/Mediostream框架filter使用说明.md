---
title: Mediostream框架filter使用说明
date: 2017-03-12 23:23:40
tags:
---

##1.链接说明

   	ms_filter_link(stream->soundread,0,stream->ec,0);
    ms_filter_link(stream->ec,0,stream->encoder,0);	
	//inputs[0] 将数据链接到0
	ms_filter_link(stream->decoder,0,stream->ec,1);
	ms_filter_link(stream->ec,1,stream->soundwrite,0);
	//inputs[1]将数据链接到1

##2.数据读取

    while((tmp=ms_queue_get(f->inputs[1]))!=NULL){
		//拿回inputs[1]数据  是从soundread来的
        log_error("save_voice tmp.pcm");
        inputlen=msgdsize(tmp);
        memcpy(tmpinput,tmp->b_rptr,inputlen);
        save_voice(voicetmp,tmpinput,inputlen);
       //save_voice(voicetmp,tmp->b_rptr,msgdsize(tmp));
        ile++;
        ce=allocb(inputlen,0);

        memcpy(ce->b_rptr, tmp->b_rptr, inputlen);

        ce->b_wptr+=inputlen;
        ms_queue_put(f->outputs[1],ce);

        freemsg(tmp);
    }

	while((im=ms_queue_get(f->inputs[0]))!=NULL){
        int len=msgdsize(im);
	//拿回inputs[0]数据   是从decoder来的



##3.接口说明

	MSFilterDesc ms_webrtc_aec_desc={
		MS_WEBRTC_AEC_ID,
		"MSWebRTCAEC",
		"Echo canceller using WebRTC library.",
		MS_FILTER_OTHER,
		"AEC",
		2,  //两进两出  ipnuts[0]和inputs[1]
		2,
		webrtc_aec_init,
		webrtc_aec_preprocess,
		webrtc_aec_process,
		webrtc_aec_postprocess,
		webrtc_aec_uninit,
		webrtc_aec_methods,
		0
	};