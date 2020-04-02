# 20春_API_人工智能与机器学习_week02
* API文档阅读介绍及计算机视觉入门（认知服务）
# 学习内容

1. 注册Azure 使用API免费服务，获取key以为获取API应用做准备
2. 使用requests，用代码取得API回复
3. 写出代码，实现输入一个图片URL，可以识别出每个人脸的年龄、性别、眼镜的，加总成绩1分；列出每个人最可能的3种情绪及其判别值再加1分。
4. 使用pandas 将返回数据用数据框展示出来。

## 作业
1. 设计一款你觉得需要人脸功能的产品。
2. 尝试使用三个API开放平台去实现（希望一定有Azure）。
3. 做出来数据结构化的表格，去阐述你的功能附加加值。
4. 放到github上边

---
我所准备的图片↓↓↓
![我所准备的图片](http://picm.bbzhi.com/mingxingbizhi/feilunhaifeilunhaibizhi/star_starhk_141010_m.jpg)
# step1：Microsoft Azure API
1.  尝试代码如下：

```
导入需要的requests模块
# 先导入为们需要的模块
import requests
import json
2、输入我们需要API网站注册的API_Key
KEY = '748eb0ea7479402aa781b3f4eea308b1'  # Replace with a valid Subscription Key here.
3、目标url [base url]
# Base URL,  Request URL中 符号?以前
BASE_URL = 'https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect' 
​
# API KEY 不要用別人的
KEY = '748eb0ea7479402aa781b3f4eea308b1'  # Replace with a valid Subscription Key here.
4、沿用API文档的示范代码,准备我们的headers和图片(数据)
# 沿用API的示范代碼，{subscription key}用KEY代入
HEADERS = {
    # Request headers
    'Content-Type': 'application/json',
    'Ocp-Apim-Subscription-Key': '{}'.format(KEY),
}
​
img_url = 'http://picm.bbzhi.com/mingxingbizhi/feilunhaifeilunhaibizhi/star_starhk_141010_m.jpg'
5、准备symbol ? 后面的数据,这里需要注意,一定要详细阅读API文档中的 “参数功能”,按照要求格式准备payload

参数功能可能有:

1、是否必要?必要的一定要准备好
2、选填的一定是功能,要根据功能需求 好好填噢
data = {
    'url': '{}'.format(img_url),
}
payload = {
    'returnFaceId': 'true',
    'returnFaceLandmarks': 'flase',
    'returnFaceAttributes': '{}'.format('age,gender,glasses,emotion'),# 年龄，性别，眼睛，情感
}
6、requests发送我们请求

注意:

详细阅读文档,注意请求方式(GET、POST、DELETE)
注意json 和字典的差异 ,str vs dict
# 坑。参考http://docs.python-requests.org/zh_CN/latest/user/quickstart.html  【更加复杂的post请求】
# 差別是 string 字串 vs. dict 字典
# Azura 使用的是 data = json.dumps(payload) 或 json=payload，data = payload 会出错

# json.dumps()，将字典格式转换成json格式
r = requests.post(BASE_URL, data=json.dumps(data), params=payload, headers=HEADERS)  # HTTP? POST? 请求参数？

r.status_code  # 查看参数回传状态
results = r.json()  # 将回传数据转化成json格式
results
# 坑。参考http://docs.python-requests.org/zh_CN/latest/user/quickstart.html  【更加复杂的post请求】
# 差別是 string 字串 vs. dict 字典
# Azura 使用的是 data = json.dumps(payload) 或 json=payload，data = payload 会出错
​
# json.dumps()，将字典格式转换成json格式
r = requests.post(BASE_URL, data=json.dumps(data), params=payload, headers=HEADERS)  # HTTP? POST? 请求参数？
​
r.status_code  # 查看参数回传状态
results = r.json()  # 将回传数据转化成json格式
results

```
2. 回传数据转化成json格式如下：
```
[{'faceId': '54d4cd29-6185-443b-96f8-e52390aebc90',
  'faceRectangle': {'top': 165, 'left': 143, 'width': 65, 'height': 65},
  'faceAttributes': {'gender': 'male',
   'age': 22.0,
   'glasses': 'NoGlasses',
   'emotion': {'anger': 0.0,
    'contempt': 0.001,
    'disgust': 0.0,
    'fear': 0.0,
    'happiness': 0.81,
    'neutral': 0.187,
    'sadness': 0.0,
    'surprise': 0.002}}},
 {'faceId': 'a54a486e-40bc-4ea4-8962-0b593cea894b',
  'faceRectangle': {'top': 172, 'left': 289, 'width': 58, 'height': 58},
  'faceAttributes': {'gender': 'male',
   'age': 22.0,
   'glasses': 'NoGlasses',
   'emotion': {'anger': 0.0,
    'contempt': 0.0,
    'disgust': 0.0,
    'fear': 0.0,
    'happiness': 0.985,
    'neutral': 0.0,
    'sadness': 0.0,
    'surprise': 0.014}}},
 {'faceId': '0f27aca8-72cb-4bf6-89b3-52ff61276638',
  'faceRectangle': {'top': 100, 'left': 425, 'width': 58, 'height': 58},
  'faceAttributes': {'gender': 'male',
   'age': 21.0,
   'glasses': 'NoGlasses',
   'emotion': {'anger': 0.0,
    'contempt': 0.0,
    'disgust': 0.0,
    'fear': 0.0,
    'happiness': 0.98,
    'neutral': 0.0,
    'sadness': 0.0,
    'surprise': 0.019}}},
 {'faceId': '0291ea00-baf3-4bd4-8e2f-1fd9cb6c4310',
  'faceRectangle': {'top': 85, 'left': 569, 'width': 56, 'height': 56},
  'faceAttributes': {'gender': 'male',
   'age': 20.0,
   'glasses': 'NoGlasses',
   'emotion': {'anger': 0.0,
    'contempt': 0.0,
    'disgust': 0.0,
    'fear': 0.0,
    'happiness': 1.0,
    'neutral': 0.0,
    'sadness': 0.0,
    'surprise': 0.0}}}]
```

#  step2: face++ Detect API
1. 尝试代码如下：
```
# 1、先导入为们需要的模块
import requests


api_secret = "KgZWE35Sr3AfTNeguHucCj7J2TqGJvBT"
# 2、输入我们API_Key
api_key = 'jbihCMcQ1xbkKYqIGQ14VujMgS3KzYe1'  # Replace with a valid Subscription Key here.


# 3、目标url
# 这里也可以使用本地图片 例如：filepath ="image/tupian.jpg"
BASE_URL = 'https://api-cn.faceplusplus.com/facepp/v3/detect' 
img_url = 'http://picm.bbzhi.com/mingxingbizhi/feilunhaifeilunhaibizhi/star_starhk_141010_m.jpg'

# 4、沿用API文档的示范代码,准备我们的headers和图片(数据)

headers = {
    'Content-Type': 'application/json',
}

# 5、准备symbol ? 后面的数据

payload = {
    "image_url":img_url,
    'api_key': api_key,
    'api_secret': api_secret,
    'return_attributes':'gender,age,smiling,emotion', 
}

#  6、requests发送我们请求
r = requests.post(BASE_URL, params=payload, headers=headers)

r.status_code
print(r.content)
```
获得requests发送我们请求内容？
```
b'{"request_id":"1585811269,9f7c8e7c-42c6-46dd-9821-e99d4bf55012","time_used":748,"faces":[{"face_token":"93a7e98d037c8057731d742daecf9fed","face_rectangle":{"top":106,"left":418,"width":62,"height":62},"attributes":{"gender":{"value":"Female"},"age":{"value":26},"smile":{"value":99.854,"threshold":50.000},"emotion":{"anger":0.134,"disgust":0.069,"fear":56.864,"happiness":40.443,"neutral":0.829,"sadness":1.231,"surprise":0.430}}},{"face_token":"c5c8c2c91482b685afac2b768a01d5e8","face_rectangle":{"top":172,"left":147,"width":62,"height":62},"attributes":{"gender":{"value":"Female"},"age":{"value":27},"smile":{"value":99.298,"threshold":50.000},"emotion":{"anger":0.082,"disgust":0.154,"fear":0.081,"happiness":85.324,"neutral":0.763,"sadness":0.240,"surprise":13.357}}},{"face_token":"e802647dbe1ccfa5c16a92c9437dc108","face_rectangle":{"top":178,"left":293,"width":61,"height":61},"attributes":{"gender":{"value":"Female"},"age":{"value":41},"smile":{"value":99.922,"threshold":50.000},"emotion":{"anger":0.038,"disgust":0.038,"fear":0.090,"happiness":91.225,"neutral":0.080,"sadness":7.916,"surprise":0.613}}},{"face_token":"e7a88fce1becff60d05e33ec4345a4f6","face_rectangle":{"top":88,"left":574,"width":57,"height":57},"attributes":{"gender":{"value":"Female"},"age":{"value":32},"smile":{"value":100.000,"threshold":50.000},"emotion":{"anger":0.004,"disgust":0.240,"fear":0.001,"happiness":99.749,"neutral":0.003,"sadness":0.002,"surprise":0.001}}}],"image_id":"jw2PfBJUVdn9qrm3y9cfHA==","face_num":4}\n'
```


2. 输入代码，尝试将回传数据转化成json格式，并且输出
```
results = r.json()
results
```
回传数据json格式输出结果如下：
```
{
  "time_used": 513,
  "faces": [
    {
      "landmark": {
        "mouth_upper_lip_left_contour2": {
          "y": 142,
          "x": 449
        },
        "mouth_upper_lip_top": {
          "y": 141,
          "x": 458
        },
        "mouth_upper_lip_left_contour1": {
          "y": 141,
          "x": 455
        },
        "left_eye_upper_left_quarter": {
          "y": 116,
          "x": 438
        },
        "left_eyebrow_lower_middle": {
          "y": 111,
          "x": 440
        },
        "mouth_upper_lip_left_contour3": {
          "y": 143,
          "x": 451
        },
        "right_eye_top": {
          "y": 112,
          "x": 466
        },
        "left_eye_bottom": {
          "y": 119,
          "x": 441
        },
        "right_eyebrow_lower_left_quarter": {
          "y": 109,
          "x": 463
        },
        "right_eye_pupil": {
          "y": 114,
          "x": 465
        },
        "mouth_lower_lip_right_contour1": {
          "y": 145,
          "x": 461
        },
        "mouth_lower_lip_right_contour3": {
          "y": 150,
          "x": 461
        },
        "mouth_lower_lip_right_contour2": {
          "y": 146,
          "x": 463
        },
        "contour_chin": {
          "y": 168,
          "x": 457
        },
        "contour_left9": {
          "y": 168,
          "x": 449
        },
        "left_eye_lower_right_quarter": {
          "y": 118,
          "x": 444
        },
        "mouth_lower_lip_top": {
          "y": 148,
          "x": 457
        },
        "right_eyebrow_upper_middle": {
          "y": 106,
          "x": 466
        },
        "left_eyebrow_left_corner": {
          "y": 113,
          "x": 430
        },
        "right_eye_bottom": {
          "y": 116,
          "x": 467
        },
        "contour_left7": {
          "y": 161,
          "x": 435
        },
        "contour_left6": {
          "y": 155,
          "x": 430
        },
        "contour_left5": {
          "y": 149,
          "x": 426
        },
        "contour_left4": {
          "y": 141,
          "x": 424
        },
        "contour_left3": {
          "y": 134,
          "x": 422
        },
        "contour_left2": {
          "y": 126,
          "x": 422
        },
        "contour_left1": {
          "y": 119,
          "x": 422
        },
        "left_eye_lower_left_quarter": {
          "y": 118,
          "x": 438
        },
        "contour_right1": {
          "y": 114,
          "x": 472
        },
        "contour_right3": {
          "y": 126,
          "x": 473
        },
        "contour_right2": {
          "y": 120,
          "x": 473
        },
        "mouth_left_corner": {
          "y": 143,
          "x": 442
        },
        "contour_right4": {
          "y": 132,
          "x": 472
        },
        "contour_right7": {
          "y": 151,
          "x": 466
        },
        "right_eyebrow_left_corner": {
          "y": 109,
          "x": 460
        },
        "nose_right": {
          "y": 131,
          "x": 465
        },
        "nose_tip": {
          "y": 131,
          "x": 459
        },
        "contour_right5": {
          "y": 139,
          "x": 471
        },
        "nose_contour_lower_middle": {
          "y": 135,
          "x": 459
        },
        "left_eyebrow_lower_left_quarter": {
          "y": 112,
          "x": 434
        },
        "mouth_lower_lip_left_contour3": {
          "y": 152,
          "x": 451
        },
        "right_eye_right_corner": {
          "y": 113,
          "x": 470
        },
        "right_eye_lower_right_quarter": {
          "y": 115,
          "x": 469
        },
        "mouth_upper_lip_right_contour2": {
          "y": 141,
          "x": 463
        },
        "right_eyebrow_lower_right_quarter": {
          "y": 108,
          "x": 469
        },
        "left_eye_left_corner": {
          "y": 118,
          "x": 435
        },
        "mouth_right_corner": {
          "y": 142,
          "x": 464
        },
        "mouth_upper_lip_right_contour3": {
          "y": 142,
          "x": 461
        },
        "right_eye_lower_left_quarter": {
          "y": 116,
          "x": 464
        },
        "left_eyebrow_right_corner": {
          "y": 111,
          "x": 451
        },
        "left_eyebrow_lower_right_quarter": {
          "y": 111,
          "x": 445
        },
        "right_eye_center": {
          "y": 114,
          "x": 466
        },
        "nose_left": {
          "y": 133,
          "x": 449
        },
        "mouth_lower_lip_left_contour1": {
          "y": 147,
          "x": 450
        },
        "left_eye_upper_right_quarter": {
          "y": 115,
          "x": 444
        },
        "right_eyebrow_lower_middle": {
          "y": 109,
          "x": 466
        },
        "left_eye_top": {
          "y": 115,
          "x": 441
        },
        "left_eye_center": {
          "y": 117,
          "x": 441
        },
        "contour_left8": {
          "y": 165,
          "x": 442
        },
        "contour_right9": {
          "y": 163,
          "x": 462
        },
        "right_eye_left_corner": {
          "y": 115,
          "x": 461
        },
        "mouth_lower_lip_bottom": {
          "y": 152,
          "x": 457
        },
        "left_eyebrow_upper_left_quarter": {
          "y": 108,
          "x": 434
        },
        "left_eye_pupil": {
          "y": 116,
          "x": 440
        },
        "right_eyebrow_upper_left_quarter": {
          "y": 107,
          "x": 462
        },
        "contour_right8": {
          "y": 157,
          "x": 464
        },
        "right_eyebrow_right_corner": {
          "y": 108,
          "x": 472
        },
        "right_eye_upper_left_quarter": {
          "y": 113,
          "x": 463
        },
        "left_eyebrow_upper_middle": {
          "y": 107,
          "x": 440
        },
        "right_eyebrow_upper_right_quarter": {
          "y": 106,
          "x": 469
        },
        "nose_contour_left1": {
          "y": 117,
          "x": 451
        },
        "nose_contour_left2": {
          "y": 128,
          "x": 451
        },
        "mouth_upper_lip_right_contour1": {
          "y": 140,
          "x": 460
        },
        "nose_contour_right1": {
          "y": 117,
          "x": 459
        },
        "nose_contour_right2": {
          "y": 127,
          "x": 463
        },
        "mouth_lower_lip_left_contour2": {
          "y": 148,
          "x": 446
        },
        "contour_right6": {
          "y": 145,
          "x": 469
        },
        "nose_contour_right3": {
          "y": 134,
          "x": 463
        },
        "nose_contour_left3": {
          "y": 135,
          "x": 454
        },
        "left_eye_right_corner": {
          "y": 117,
          "x": 447
        },
        "left_eyebrow_upper_right_quarter": {
          "y": 108,
          "x": 446
        },
        "right_eye_upper_right_quarter": {
          "y": 112,
          "x": 469
        },
        "mouth_upper_lip_bottom": {
          "y": 143,
          "x": 458
        }
      },
      "attributes": {
        "gender": {
          "value": "Female"
        },
        "age": {
          "value": 26
        },
        "eyestatus": {
          "left_eye_status": {
            "normal_glass_eye_open": 0.111,
            "no_glass_eye_close": 0,
            "occlusion": 0.001,
            "no_glass_eye_open": 99.888,
            "normal_glass_eye_close": 0,
            "dark_glasses": 0
          },
          "right_eye_status": {
            "normal_glass_eye_open": 3.067,
            "no_glass_eye_close": 0,
            "occlusion": 0.032,
            "no_glass_eye_open": 96.894,
            "normal_glass_eye_close": 0.004,
            "dark_glasses": 0.003
          }
        },
        "glass": {
          "value": "None"
        },
        "headpose": {
          "yaw_angle": -17.108103,
          "pitch_angle": 17.205276,
          "roll_angle": -1.0454177
        },
        "blur": {
          "blurness": {
            "threshold": 50,
            "value": 18.528
          },
          "motionblur": {
            "threshold": 50,
            "value": 18.528
          },
          "gaussianblur": {
            "threshold": 50,
            "value": 18.528
          }
        },
        "smile": {
          "threshold": 50,
          "value": 99.855
        },
        "facequality": {
          "threshold": 70.1,
          "value": 13.922
        }
      },
      "face_rectangle": {
        "width": 62,
        "top": 106,
        "left": 418,
        "height": 62
      },
      "face_token": "2a6d2685b4260e94a8bc4c2c71faae00"
    },
    {
      "landmark": {
        "mouth_upper_lip_left_contour2": {
          "y": 210,
          "x": 167
        },
        "mouth_upper_lip_top": {
          "y": 210,
          "x": 174
        },
        "mouth_upper_lip_left_contour1": {
          "y": 210,
          "x": 171
        },
        "left_eye_upper_left_quarter": {
          "y": 179,
          "x": 159
        },
        "left_eyebrow_lower_middle": {
          "y": 174,
          "x": 160
        },
        "mouth_upper_lip_left_contour3": {
          "y": 211,
          "x": 169
        },
        "right_eye_top": {
          "y": 181,
          "x": 190
        },
        "left_eye_bottom": {
          "y": 183,
          "x": 161
        },
        "right_eyebrow_lower_left_quarter": {
          "y": 176,
          "x": 188
        },
        "right_eye_pupil": {
          "y": 183,
          "x": 190
        },
        "mouth_lower_lip_right_contour1": {
          "y": 217,
          "x": 180
        },
        "mouth_lower_lip_right_contour3": {
          "y": 222,
          "x": 179
        },
        "mouth_lower_lip_right_contour2": {
          "y": 218,
          "x": 183
        },
        "contour_chin": {
          "y": 234,
          "x": 174
        },
        "contour_left9": {
          "y": 231,
          "x": 166
        },
        "left_eye_lower_right_quarter": {
          "y": 183,
          "x": 164
        },
        "mouth_lower_lip_top": {
          "y": 219,
          "x": 174
        },
        "right_eyebrow_upper_middle": {
          "y": 174,
          "x": 192
        },
        "left_eyebrow_left_corner": {
          "y": 173,
          "x": 152
        },
        "right_eye_bottom": {
          "y": 185,
          "x": 190
        },
        "contour_left7": {
          "y": 220,
          "x": 158
        },
        "contour_left6": {
          "y": 214,
          "x": 155
        },
        "contour_left5": {
          "y": 207,
          "x": 152
        },
        "contour_left4": {
          "y": 200,
          "x": 151
        },
        "contour_left3": {
          "y": 194,
          "x": 150
        },
        "contour_left2": {
          "y": 187,
          "x": 150
        },
        "contour_left1": {
          "y": 180,
          "x": 150
        },
        "left_eye_lower_left_quarter": {
          "y": 182,
          "x": 158
        },
        "contour_right1": {
          "y": 186,
          "x": 210
        },
        "contour_right3": {
          "y": 200,
          "x": 209
        },
        "contour_right2": {
          "y": 193,
          "x": 210
        },
        "mouth_left_corner": {
          "y": 212,
          "x": 163
        },
        "contour_right4": {
          "y": 208,
          "x": 207
        },
        "contour_right7": {
          "y": 226,
          "x": 195
        },
        "right_eyebrow_left_corner": {
          "y": 175,
          "x": 183
        },
        "nose_right": {
          "y": 202,
          "x": 183
        },
        "nose_tip": {
          "y": 200,
          "x": 172
        },
        "contour_right5": {
          "y": 215,
          "x": 204
        },
        "nose_contour_lower_middle": {
          "y": 205,
          "x": 172
        },
        "left_eyebrow_lower_left_quarter": {
          "y": 173,
          "x": 156
        },
        "mouth_lower_lip_left_contour3": {
          "y": 221,
          "x": 168
        },
        "right_eye_right_corner": {
          "y": 184,
          "x": 196
        },
        "right_eye_lower_right_quarter": {
          "y": 185,
          "x": 193
        },
        "mouth_upper_lip_right_contour2": {
          "y": 211,
          "x": 181
        },
        "right_eyebrow_lower_right_quarter": {
          "y": 177,
          "x": 196
        },
        "left_eye_left_corner": {
          "y": 180,
          "x": 156
        },
        "mouth_right_corner": {
          "y": 213,
          "x": 185
        },
        "mouth_upper_lip_right_contour3": {
          "y": 212,
          "x": 179
        },
        "right_eye_lower_left_quarter": {
          "y": 185,
          "x": 187
        },
        "left_eyebrow_right_corner": {
          "y": 175,
          "x": 168
        },
        "left_eyebrow_lower_right_quarter": {
          "y": 175,
          "x": 164
        },
        "right_eye_center": {
          "y": 184,
          "x": 190
        },
        "nose_left": {
          "y": 200,
          "x": 164
        },
        "mouth_lower_lip_left_contour1": {
          "y": 216,
          "x": 168
        },
        "left_eye_upper_right_quarter": {
          "y": 180,
          "x": 165
        },
        "right_eyebrow_lower_middle": {
          "y": 177,
          "x": 192
        },
        "left_eye_top": {
          "y": 179,
          "x": 162
        },
        "left_eye_center": {
          "y": 181,
          "x": 161
        },
        "contour_left8": {
          "y": 226,
          "x": 162
        },
        "contour_right9": {
          "y": 234,
          "x": 182
        },
        "right_eye_left_corner": {
          "y": 184,
          "x": 184
        },
        "mouth_lower_lip_bottom": {
          "y": 223,
          "x": 173
        },
        "left_eyebrow_upper_left_quarter": {
          "y": 171,
          "x": 156
        },
        "left_eye_pupil": {
          "y": 180,
          "x": 162
        },
        "right_eyebrow_upper_left_quarter": {
          "y": 174,
          "x": 187
        },
        "contour_right8": {
          "y": 231,
          "x": 189
        },
        "right_eyebrow_right_corner": {
          "y": 176,
          "x": 200
        },
        "right_eye_upper_left_quarter": {
          "y": 182,
          "x": 186
        },
        "left_eyebrow_upper_middle": {
          "y": 171,
          "x": 161
        },
        "right_eyebrow_upper_right_quarter": {
          "y": 174,
          "x": 196
        },
        "nose_contour_left1": {
          "y": 183,
          "x": 170
        },
        "nose_contour_left2": {
          "y": 195,
          "x": 167
        },
        "mouth_upper_lip_right_contour1": {
          "y": 210,
          "x": 177
        },
        "nose_contour_right1": {
          "y": 184,
          "x": 180
        },
        "nose_contour_right2": {
          "y": 197,
          "x": 181
        },
        "mouth_lower_lip_left_contour2": {
          "y": 217,
          "x": 165
        },
        "contour_right6": {
          "y": 221,
          "x": 200
        },
        "nose_contour_right3": {
          "y": 204,
          "x": 178
        },
        "nose_contour_left3": {
          "y": 203,
          "x": 168
        },
        "left_eye_right_corner": {
          "y": 183,
          "x": 167
        },
        "left_eyebrow_upper_right_quarter": {
          "y": 172,
          "x": 165
        },
        "right_eye_upper_right_quarter": {
          "y": 182,
          "x": 193
        },
        "mouth_upper_lip_bottom": {
          "y": 212,
          "x": 174
        }
      },
      "attributes": {
        "gender": {
          "value": "Female"
        },
        "age": {
          "value": 27
        },
        "eyestatus": {
          "left_eye_status": {
            "normal_glass_eye_open": 6.096,
            "no_glass_eye_close": 0,
            "occlusion": 0.001,
            "no_glass_eye_open": 93.899,
            "normal_glass_eye_close": 0.003,
            "dark_glasses": 0
          },
          "right_eye_status": {
            "normal_glass_eye_open": 5.659,
            "no_glass_eye_close": 0,
            "occlusion": 0.012,
            "no_glass_eye_open": 94.328,
            "normal_glass_eye_close": 0.001,
            "dark_glasses": 0
          }
        },
        "glass": {
          "value": "None"
        },
        "headpose": {
          "yaw_angle": 13.349398,
          "pitch_angle": 10.8344145,
          "roll_angle": 2.6650326
        },
        "blur": {
          "blurness": {
            "threshold": 50,
            "value": 1.384
          },
          "motionblur": {
            "threshold": 50,
            "value": 1.384
          },
          "gaussianblur": {
            "threshold": 50,
            "value": 1.384
          }
        },
        "smile": {
          "threshold": 50,
          "value": 99.292
        },
        "facequality": {
          "threshold": 70.1,
          "value": 57.474
        }
      },
      "face_rectangle": {
        "width": 62,
        "top": 172,
        "left": 147,
        "height": 62
      },
      "face_token": "6cc418e10c5936d1fd4a184ef701a3b6"
    },
    {
      "landmark": {
        "mouth_upper_lip_left_contour2": {
          "y": 213,
          "x": 310
        },
        "mouth_upper_lip_top": {
          "y": 213,
          "x": 315
        },
        "mouth_upper_lip_left_contour1": {
          "y": 212,
          "x": 313
        },
        "left_eye_upper_left_quarter": {
          "y": 185,
          "x": 304
        },
        "left_eyebrow_lower_middle": {
          "y": 181,
          "x": 306
        },
        "mouth_upper_lip_left_contour3": {
          "y": 214,
          "x": 311
        },
        "right_eye_top": {
          "y": 188,
          "x": 331
        },
        "left_eye_bottom": {
          "y": 188,
          "x": 306
        },
        "right_eyebrow_lower_left_quarter": {
          "y": 183,
          "x": 329
        },
        "right_eye_pupil": {
          "y": 189,
          "x": 332
        },
        "mouth_lower_lip_right_contour1": {
          "y": 223,
          "x": 323
        },
        "mouth_lower_lip_right_contour3": {
          "y": 227,
          "x": 322
        },
        "mouth_lower_lip_right_contour2": {
          "y": 223,
          "x": 326
        },
        "contour_chin": {
          "y": 239,
          "x": 317
        },
        "contour_left9": {
          "y": 235,
          "x": 311
        },
        "left_eye_lower_right_quarter": {
          "y": 188,
          "x": 309
        },
        "mouth_lower_lip_top": {
          "y": 224,
          "x": 316
        },
        "right_eyebrow_upper_middle": {
          "y": 179,
          "x": 334
        },
        "left_eyebrow_left_corner": {
          "y": 181,
          "x": 300
        },
        "right_eye_bottom": {
          "y": 191,
          "x": 331
        },
        "contour_left7": {
          "y": 224,
          "x": 306
        },
        "contour_left6": {
          "y": 218,
          "x": 303
        },
        "contour_left5": {
          "y": 213,
          "x": 301
        },
        "contour_left4": {
          "y": 207,
          "x": 299
        },
        "contour_left3": {
          "y": 200,
          "x": 298
        },
        "contour_left2": {
          "y": 194,
          "x": 299
        },
        "contour_left1": {
          "y": 188,
          "x": 300
        },
        "left_eye_lower_left_quarter": {
          "y": 187,
          "x": 304
        },
        "contour_right1": {
          "y": 193,
          "x": 351
        },
        "contour_right3": {
          "y": 207,
          "x": 351
        },
        "contour_right2": {
          "y": 200,
          "x": 352
        },
        "mouth_left_corner": {
          "y": 215,
          "x": 309
        },
        "contour_right4": {
          "y": 214,
          "x": 350
        },
        "contour_right7": {
          "y": 232,
          "x": 337
        },
        "right_eyebrow_left_corner": {
          "y": 182,
          "x": 324
        },
        "nose_right": {
          "y": 206,
          "x": 324
        },
        "nose_tip": {
          "y": 204,
          "x": 313
        },
        "contour_right5": {
          "y": 221,
          "x": 347
        },
        "nose_contour_lower_middle": {
          "y": 208,
          "x": 314
        },
        "left_eyebrow_lower_left_quarter": {
          "y": 181,
          "x": 303
        },
        "mouth_lower_lip_left_contour3": {
          "y": 225,
          "x": 311
        },
        "right_eye_right_corner": {
          "y": 191,
          "x": 336
        },
        "right_eye_lower_right_quarter": {
          "y": 191,
          "x": 334
        },
        "mouth_upper_lip_right_contour2": {
          "y": 215,
          "x": 324
        },
        "right_eyebrow_lower_right_quarter": {
          "y": 184,
          "x": 338
        },
        "left_eye_left_corner": {
          "y": 186,
          "x": 302
        },
        "mouth_right_corner": {
          "y": 218,
          "x": 329
        },
        "mouth_upper_lip_right_contour3": {
          "y": 216,
          "x": 322
        },
        "right_eye_lower_left_quarter": {
          "y": 191,
          "x": 328
        },
        "left_eyebrow_right_corner": {
          "y": 182,
          "x": 312
        },
        "left_eyebrow_lower_right_quarter": {
          "y": 182,
          "x": 309
        },
        "right_eye_center": {
          "y": 190,
          "x": 331
        },
        "nose_left": {
          "y": 204,
          "x": 308
        },
        "mouth_lower_lip_left_contour1": {
          "y": 221,
          "x": 312
        },
        "left_eye_upper_right_quarter": {
          "y": 186,
          "x": 309
        },
        "right_eyebrow_lower_middle": {
          "y": 183,
          "x": 334
        },
        "left_eye_top": {
          "y": 185,
          "x": 307
        },
        "left_eye_center": {
          "y": 187,
          "x": 307
        },
        "contour_left8": {
          "y": 230,
          "x": 308
        },
        "contour_right9": {
          "y": 239,
          "x": 324
        },
        "right_eye_left_corner": {
          "y": 190,
          "x": 325
        },
        "mouth_lower_lip_bottom": {
          "y": 228,
          "x": 316
        },
        "left_eyebrow_upper_left_quarter": {
          "y": 179,
          "x": 303
        },
        "left_eye_pupil": {
          "y": 186,
          "x": 308
        },
        "right_eyebrow_upper_left_quarter": {
          "y": 180,
          "x": 329
        },
        "contour_right8": {
          "y": 236,
          "x": 331
        },
        "right_eyebrow_right_corner": {
          "y": 185,
          "x": 342
        },
        "right_eye_upper_left_quarter": {
          "y": 188,
          "x": 328
        },
        "left_eyebrow_upper_middle": {
          "y": 179,
          "x": 306
        },
        "right_eyebrow_upper_right_quarter": {
          "y": 181,
          "x": 339
        },
        "nose_contour_left1": {
          "y": 189,
          "x": 314
        },
        "nose_contour_left2": {
          "y": 200,
          "x": 310
        },
        "mouth_upper_lip_right_contour1": {
          "y": 213,
          "x": 317
        },
        "nose_contour_right1": {
          "y": 190,
          "x": 322
        },
        "nose_contour_right2": {
          "y": 201,
          "x": 322
        },
        "mouth_lower_lip_left_contour2": {
          "y": 221,
          "x": 310
        },
        "contour_right6": {
          "y": 227,
          "x": 342
        },
        "nose_contour_right3": {
          "y": 208,
          "x": 319
        },
        "nose_contour_left3": {
          "y": 207,
          "x": 311
        },
        "left_eye_right_corner": {
          "y": 188,
          "x": 311
        },
        "left_eyebrow_upper_right_quarter": {
          "y": 180,
          "x": 310
        },
        "right_eye_upper_right_quarter": {
          "y": 189,
          "x": 334
        },
        "mouth_upper_lip_bottom": {
          "y": 215,
          "x": 315
        }
      },
      "attributes": {
        "gender": {
          "value": "Female"
        },
        "age": {
          "value": 41
        },
        "eyestatus": {
          "left_eye_status": {
            "normal_glass_eye_open": 0.076,
            "no_glass_eye_close": 0,
            "occlusion": 0.007,
            "no_glass_eye_open": 99.916,
            "normal_glass_eye_close": 0.001,
            "dark_glasses": 0
          },
          "right_eye_status": {
            "normal_glass_eye_open": 1.767,
            "no_glass_eye_close": 0.006,
            "occlusion": 0.02,
            "no_glass_eye_open": 98.2,
            "normal_glass_eye_close": 0.007,
            "dark_glasses": 0.001
          }
        },
        "glass": {
          "value": "None"
        },
        "headpose": {
          "yaw_angle": 25.658672,
          "pitch_angle": 5.02963,
          "roll_angle": 1.4602504
        },
        "blur": {
          "blurness": {
            "threshold": 50,
            "value": 10.588
          },
          "motionblur": {
            "threshold": 50,
            "value": 10.588
          },
          "gaussianblur": {
            "threshold": 50,
            "value": 10.588
          }
        },
        "smile": {
          "threshold": 50,
          "value": 99.926
        },
        "facequality": {
          "threshold": 70.1,
          "value": 0.763
        }
      },
      "face_rectangle": {
        "width": 61,
        "top": 178,
        "left": 293,
        "height": 61
      },
      "face_token": "f9e0c21c8b625d43c1a730db221ae514"
    },
    {
      "landmark": {
        "mouth_upper_lip_left_contour2": {
          "y": 125,
          "x": 589
        },
        "mouth_upper_lip_top": {
          "y": 125,
          "x": 595
        },
        "mouth_upper_lip_left_contour1": {
          "y": 124,
          "x": 593
        },
        "left_eye_upper_left_quarter": {
          "y": 100,
          "x": 581
        },
        "left_eyebrow_lower_middle": {
          "y": 96,
          "x": 581
        },
        "mouth_upper_lip_left_contour3": {
          "y": 126,
          "x": 591
        },
        "right_eye_top": {
          "y": 98,
          "x": 611
        },
        "left_eye_bottom": {
          "y": 103,
          "x": 584
        },
        "right_eyebrow_lower_left_quarter": {
          "y": 93,
          "x": 606
        },
        "right_eye_pupil": {
          "y": 99,
          "x": 612
        },
        "mouth_lower_lip_right_contour1": {
          "y": 128,
          "x": 604
        },
        "mouth_lower_lip_right_contour3": {
          "y": 133,
          "x": 603
        },
        "mouth_lower_lip_right_contour2": {
          "y": 129,
          "x": 608
        },
        "contour_chin": {
          "y": 145,
          "x": 598
        },
        "contour_left9": {
          "y": 143,
          "x": 592
        },
        "left_eye_lower_right_quarter": {
          "y": 103,
          "x": 586
        },
        "mouth_lower_lip_top": {
          "y": 130,
          "x": 596
        },
        "right_eyebrow_upper_middle": {
          "y": 90,
          "x": 611
        },
        "left_eyebrow_left_corner": {
          "y": 96,
          "x": 575
        },
        "right_eye_bottom": {
          "y": 101,
          "x": 611
        },
        "contour_left7": {
          "y": 135,
          "x": 584
        },
        "contour_left6": {
          "y": 130,
          "x": 581
        },
        "contour_left5": {
          "y": 125,
          "x": 578
        },
        "contour_left4": {
          "y": 120,
          "x": 576
        },
        "contour_left3": {
          "y": 115,
          "x": 574
        },
        "contour_left2": {
          "y": 109,
          "x": 574
        },
        "contour_left1": {
          "y": 103,
          "x": 574
        },
        "left_eye_lower_left_quarter": {
          "y": 102,
          "x": 581
        },
        "contour_right1": {
          "y": 100,
          "x": 631
        },
        "contour_right3": {
          "y": 113,
          "x": 631
        },
        "contour_right2": {
          "y": 106,
          "x": 631
        },
        "mouth_left_corner": {
          "y": 125,
          "x": 586
        },
        "contour_right4": {
          "y": 120,
          "x": 630
        },
        "contour_right7": {
          "y": 137,
          "x": 619
        },
        "right_eyebrow_left_corner": {
          "y": 94,
          "x": 602
        },
        "nose_right": {
          "y": 117,
          "x": 604
        },
        "nose_tip": {
          "y": 116,
          "x": 593
        },
        "contour_right5": {
          "y": 126,
          "x": 628
        },
        "nose_contour_lower_middle": {
          "y": 120,
          "x": 595
        },
        "left_eyebrow_lower_left_quarter": {
          "y": 96,
          "x": 578
        },
        "mouth_lower_lip_left_contour3": {
          "y": 133,
          "x": 591
        },
        "right_eye_right_corner": {
          "y": 100,
          "x": 617
        },
        "right_eye_lower_right_quarter": {
          "y": 101,
          "x": 614
        },
        "mouth_upper_lip_right_contour2": {
          "y": 124,
          "x": 605
        },
        "right_eyebrow_lower_right_quarter": {
          "y": 93,
          "x": 616
        },
        "left_eye_left_corner": {
          "y": 102,
          "x": 579
        },
        "mouth_right_corner": {
          "y": 124,
          "x": 612
        },
        "mouth_upper_lip_right_contour3": {
          "y": 126,
          "x": 603
        },
        "right_eye_lower_left_quarter": {
          "y": 101,
          "x": 608
        },
        "left_eyebrow_right_corner": {
          "y": 96,
          "x": 589
        },
        "left_eyebrow_lower_right_quarter": {
          "y": 96,
          "x": 585
        },
        "right_eye_center": {
          "y": 100,
          "x": 611
        },
        "nose_left": {
          "y": 117,
          "x": 588
        },
        "mouth_lower_lip_left_contour1": {
          "y": 129,
          "x": 590
        },
        "left_eye_upper_right_quarter": {
          "y": 100,
          "x": 586
        },
        "right_eyebrow_lower_middle": {
          "y": 93,
          "x": 611
        },
        "left_eye_top": {
          "y": 99,
          "x": 584
        },
        "left_eye_center": {
          "y": 102,
          "x": 584
        },
        "contour_left8": {
          "y": 139,
          "x": 588
        },
        "contour_right9": {
          "y": 144,
          "x": 606
        },
        "right_eye_left_corner": {
          "y": 101,
          "x": 606
        },
        "mouth_lower_lip_bottom": {
          "y": 134,
          "x": 597
        },
        "left_eyebrow_upper_left_quarter": {
          "y": 93,
          "x": 577
        },
        "left_eye_pupil": {
          "y": 101,
          "x": 584
        },
        "right_eyebrow_upper_left_quarter": {
          "y": 91,
          "x": 606
        },
        "contour_right8": {
          "y": 141,
          "x": 613
        },
        "right_eyebrow_right_corner": {
          "y": 93,
          "x": 621
        },
        "right_eye_upper_left_quarter": {
          "y": 99,
          "x": 608
        },
        "left_eyebrow_upper_middle": {
          "y": 92,
          "x": 581
        },
        "right_eyebrow_upper_right_quarter": {
          "y": 90,
          "x": 616
        },
        "nose_contour_left1": {
          "y": 102,
          "x": 592
        },
        "nose_contour_left2": {
          "y": 113,
          "x": 589
        },
        "mouth_upper_lip_right_contour1": {
          "y": 124,
          "x": 598
        },
        "nose_contour_right1": {
          "y": 102,
          "x": 600
        },
        "nose_contour_right2": {
          "y": 113,
          "x": 602
        },
        "mouth_lower_lip_left_contour2": {
          "y": 129,
          "x": 588
        },
        "contour_right6": {
          "y": 132,
          "x": 624
        },
        "nose_contour_right3": {
          "y": 119,
          "x": 600
        },
        "nose_contour_left3": {
          "y": 119,
          "x": 591
        },
        "left_eye_right_corner": {
          "y": 102,
          "x": 589
        },
        "left_eyebrow_upper_right_quarter": {
          "y": 93,
          "x": 585
        },
        "right_eye_upper_right_quarter": {
          "y": 98,
          "x": 614
        },
        "mouth_upper_lip_bottom": {
          "y": 127,
          "x": 596
        }
      },
      "attributes": {
        "gender": {
          "value": "Female"
        },
        "age": {
          "value": 32
        },
        "eyestatus": {
          "left_eye_status": {
            "normal_glass_eye_open": 1.331,
            "no_glass_eye_close": 0,
            "occlusion": 0.001,
            "no_glass_eye_open": 98.666,
            "normal_glass_eye_close": 0.001,
            "dark_glasses": 0.001
          },
          "right_eye_status": {
            "normal_glass_eye_open": 0.29,
            "no_glass_eye_close": 0.001,
            "occlusion": 0,
            "no_glass_eye_open": 99.709,
            "normal_glass_eye_close": 0,
            "dark_glasses": 0
          }
        },
        "glass": {
          "value": "None"
        },
        "headpose": {
          "yaw_angle": 13.136379,
          "pitch_angle": 8.271756,
          "roll_angle": -5.110254
        },
        "blur": {
          "blurness": {
            "threshold": 50,
            "value": 0.525
          },
          "motionblur": {
            "threshold": 50,
            "value": 0.525
          },
          "gaussianblur": {
            "threshold": 50,
            "value": 0.525
          }
        },
        "smile": {
          "threshold": 50,
          "value": 100
        },
        "facequality": {
          "threshold": 70.1,
          "value": 62.972
        }
      },
      "face_rectangle": {
        "width": 57,
        "top": 88,
        "left": 574,
        "height": 57
      },
      "face_token": "aef37ec24408031e4d291df5b87d5bb7"
    }
  ],
  "image_id": "jw2PfBJUVdn9qrm3y9cfHA==",
  "request_id": "1585755469,44b3fe69-d0bd-4ab5-a447-af056ec64687",
  "face_num": 4
}
```

---
#### 设计一款你觉得会使用到人脸识别功能的产品
+ 其实在今年寒假（春节前），我有在银行兼职工作。工作的主要内容是：给前来银行取钱的老爷爷老奶奶进行相关操作指导。在这过程中，有过好几次，有的高龄老人，子女外出工作，独自一人前往银行办理业务，但他们老人家的听力不好，而且像输入密码、手动签名等操作根本无法正常进行。这不仅对其个人办理业务有影响，往往很大程度上耽误了接下来需要的办理业务的客户的进度。
+ 这种时候，我觉得可以将“**人脸识别技术 **应用到**针对高龄老人群体的银行业务办理**上，**对客户实施分类识别挂号**，对于高龄老人办理基础简单的业务，可以**采取人脸识别进行相关信息确认**，办理过程可采集办理过程的对话语音、人脸信息存档等措施进行业务办理存证。
+ 一定程度上满足高龄老人的办理需求，也极大程度上推进银行事务的办理进度。
+ 启迪案例：[北京地铁将应用人脸识别技术对乘客实施分类安检|北京地铁_新浪新闻](https://news.sina.com.cn/s/2019-10-29/doc-iicezuev5615496.shtml)

