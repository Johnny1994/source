---
title: 使用ffprobe进行音视频流的分析（一）
date: 2017-03-13 10:09:12
categories: 多媒体
tags: 博客
---
音视频流的结构的分析对进行音视频的处理或者对直播过程中的直播流的卡顿等情况的分析处理起到了至关重要的作用。目前刚刚开始接触这块儿，还是个小白，这里根据平时使用到的一些ffprobe的命令进行一些总结，以作备忘：

ffprobe常用的参数比较多，如果想知道具体的可以使用ffprobe --help来查看一些详细的命令。
我目前涉及到的主要是查看视频流的时间戳、编码格式，主要用得到的命令如下：

1. ** 使用 -show_frames 参数查看视频中的帧信息：**
        {
            "media_type": "video",
            "stream_index": 1,
            "key_frame": 0,
            "pkt_pts": 27275,
            "pkt_pts_time": "45.458333",
            "pkt_dts": 27274,
            "pkt_dts_time": "45.456667",
            "best_effort_timestamp": 27275,
            "best_effort_timestamp_time": "45.458333",
            "pkt_duration": 20,
            "pkt_duration_time": "0.033333",
            "pkt_pos": "4519791",
            "pkt_size": "970",
            "width": 568,
            "height": 320,
            "pix_fmt": "yuv420p",
            "pict_type": "B",
            "coded_picture_number": 1364,
            "display_picture_number": 0,
            "interlaced_frame": 0,
            "top_field_first": 0,
            "repeat_pict": 0
        },
-show_frames打印出来的信息都是帧相关的，包括视频帧和音频帧，其中主要的数据及其含义如下：
> key_frame：是否是关键帧
pkt_pts：帧的pts数值
pkt_pts_time：通过time_base计算出来的显示时间
pkt_dts：帧的dts数值
pkt_dts_time：通过time_base计算出来的dts时间
pict_type：帧类型（I、B、P）
2.  **使用 -show_streams 参数查看视频中的流信息：**
            "index": 1,
            "codec_name": "h264",
            "codec_long_name": "H.264 / AVC / MPEG-4 AVC / MPEG-4 part 10",
            "profile": "Main",
            "codec_type": "video",
            "codec_time_base": "464/27825",
            "codec_tag_string": "avc1",
            "codec_tag": "0x31637661",
            "width": 568,
            "height": 320,
            "coded_width": 568,
            "coded_height": 320,
            "has_b_frames": 0,
            "sample_aspect_ratio": "0:1",
            "display_aspect_ratio": "0:1",
            "pix_fmt": "yuv420p",
            "level": 30,
            "color_range": "tv",
            "color_space": "bt709",
            "color_transfer": "bt709",
            "color_primaries": "bt709",
            "chroma_location": "left",
            "refs": 1,
            "is_avc": "true",
            "nal_length_size": "4",
            "r_frame_rate": "30000/1001",
            "avg_frame_rate": "27825/928",
            "time_base": "1/600",
            "start_pts": 0,
            "start_time": "0.000000",
            "duration_ts": 29696,
            "duration": "49.493333",
            "bit_rate": "705282",
            "bits_per_raw_sample": "8",
            "nb_frames": "1484",
            "disposition": {
                "default": 1,
                "dub": 0,
                "original": 0,
                "comment": 0,
                "lyrics": 0,
                "karaoke": 0,
                "forced": 0,
                "hearing_impaired": 0,
                "visual_impaired": 0,
                "clean_effects": 0,
                "attached_pic": 0,
                "timed_thumbnails": 0
            },
            "tags": {
                "creation_time": "2016-12-22T03:35:39.000000Z",
                "language": "und",
                "handler_name": "Core Media Video"
            }
以上便是打印出来的信息，主要的就是编码格式、原始数据格式、time_base和码率等信息是使用较多的。
3. ** 使用 -show_packets 参数查看包信息：**
        {
            "codec_type": "video",
            "stream_index": 1,
            "pts": 28676,
            "pts_time": "47.793333",
            "dts": 28675,
            "dts_time": "47.791667",
            "duration": 20,
            "duration_time": "0.033333",
            "size": "1199",
            "pos": "4737832",
            "flags": "__"
        },
同上，主要输出的都是关于流类型和pts、dts等信息。

上面的参数可以获得音视频相关的各种参数，但是显示的可能比较乱，所以可以使用下面的参数进行输出的格式化：
> -of 或者 -print_format + compact/csv/flat/ini/json/xml

同时，可以通过使用如下参数进行视频流或者音频流的选择：
> -select_streams + a(音频) / v(视频)

利用好这个工具，可以在进行音视频编解码的时候对时间戳等信息进行更好的校对；同时在进行音视频流卡顿的分析的时候也很有用处，比如可以通过观察pts等信息查看是否有时间戳回退等问题的存在。

后续如果有新的收获还会继续更新。