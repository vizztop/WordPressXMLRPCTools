## 遇到问题

客服系统实现仿微信输入框粘贴图片发送

### vue 维恩图

 [ venn.js](https://github.com/benfred/venn.js)

### 获取视频封面

使用js获取视频的首帧图片

```js
/**
 * 获取视频封面
 * @param url
 * @returns {Promise<unknown>}
 */
getVideoBase64(url) {
    return new Promise((resolve, reject) => {

        let dataURL = '';
        let video = document.createElement("video");

        // 处理跨域
        video.setAttribute('crossOrigin', 'anonymous');
        video.setAttribute('src', url);
        video.setAttribute('width', 400);
        video.setAttribute('height', 240);
        video.setAttribute('autoplay', 'autoplay');
        video.pause();
        video.addEventListener('loadeddata', () => {
            // 视频时长video.duration;
            let canvas = document.createElement("canvas"),
                //canvas的尺寸和图片一样
                width = video.width, 
                height = video.height;

            canvas.width = width;
            canvas.height = height;
             // 绘制canvas
            canvas.getContext("2d").drawImage(video, 0, 0, width, height);
            // 转换为base64
            dataURL = canvas.toDataURL('image/jpeg'); 
            resolve(dataURL);
        });
    });
}
```

### Vue上传视频前本地预览视频

使用URL.createObjectURL方法将文件对象转为可访问url  [文档参考](https://developer.mozilla.org/zh-CN/docs/Web/API/URL/createObjectURL)

这里只是简单列出使用方法，可根据实际情况应用

另外还可以使用FileReader

```html
<!--html-->
<el-upload
    class="upload-demo"
    action="noAction"
    ref="uploadVideo"
    accept="video/MP4,video/ogg,video/mkv,video/avi,video/rmvb"
    :limit="1"
    :show-file-list="false"
    :http-request="handleUploadVideo"
>
    <div class="uploader-customer">
        <i class="el-icon-plus avatar-uploader-icon"/>
        <div class="upload-text">添加视频</div>
    </div>
</el-upload>
```

```js
/**
 * 上传视频
 * @param event
 */
handleUploadVideo(event) {

    // event.file 要上传的文件对象
    // videoUrl可以作为video 的src 也可以直接使用window.open()打开播放
    let videoUrl = URL.createObjectURL(event.file);
    
    // let video = new FormData();
	// video.append('file', event.file);

    // 上传方法
    // this.$upload({
    //    url: '',
    //    data: video
    // });
},
```

### vue-router history模式返回不生效

```js
/**
 * 混入 返回上一页
 */
export const back = {
    data () {
        return {
            pathName: ''
        };
    },
    beforeRouteEnter(to, from, next) {
        next(vm => {
            vm.pathName = from.name;
        });
    },
    destroyed(){
        window.removeEventListener('popstate', this.fnGoBack, false);
    },
    mounted () {
        this.fnRecord();
    },
    methods: {
        fnRecord () {
            if (window.history && window.history.pushState) {
                history.pushState(null, null, document.URL);
                window.addEventListener('popstate', this.fnGoBack, false);
            }
        },
        fnGoBack  () {
            this.$router.replace({ name: this.pathName });
        }
    }
};
```

### JS工具函数  -  格式化文件大小

```js
formatFileSize(fileSize, idx = 0) {
	const units = ["B", "KB", "MB", "GB"];
	if (fileSize < 1024 || idx === units.length - 1) {
		return fileSize.toFixed(1) + units[idx];
	}
	return formatFileSize(fileSize / 1024, ++idx);
}
```

### 数据类型判断

Object.prototype.toString.call()返回的数据格式为 `[object Object]`类型，然后用slice截取第8位到倒一位，得到结果为 `Object`

```js
var _toString = Object.prototype.toString;
function toRawType (value) {
  return _toString.call(value).slice(8, -1)
}
```

运行结果测试

```js
toRawType({}) //  Object 
toRawType([])  // Array    
toRawType(true) // Boolean
toRawType(undefined) // Undefined
toRawType(null) // Null
toRawType(function(){}) // Function
```

### 下载图片

```js
/**
 * @description 下载二维码
 * @param src
 * @param imgName
 */
downLoadCode(src, imgName) {
    let canvas = document.createElement('canvas');
    let img = document.createElement('img');

    img.onload = function (e) {
        canvas.width = img.width;
        canvas.height = img.height;
        let context = canvas.getContext('2d');

        context.drawImage(img, 0, 0, img.width, img.height);
        canvas.getContext('2d').drawImage(img, 0, 0, img.width, img.height);
        canvas.toBlob((blob) => {
            let link = document.createElement('a');

            link.href = window.URL.createObjectURL(blob);
            link.download = imgName.split('.')[0];
            link.click();
        },
        "image/png");
    };
    img.crossOrigin = "Anonymous";
    img.src = src;
},
```

### 早早聊大会

第十八届前端早早聊大会录播及 PPT 来啦，密码：qtm2,https://www.yuque.com/docs/share/d879960a-de7a-4d2a-8dea-df3c74ddbd1f

\---

历届录播 PPT-密码不定期更新: 
第十七期搞框架| 密码:sgi3, https://www.yuque.com/docs/share/7b894140-b99f-4b1e-a934-f879db71a2da
第十六期搞组件化|密码:wf8o, https://www.yuque.com/docs/share/0156379c-c1e5-4c27-9ed7-091d3593eb6b 
第十五期搞报表|密码:ctg5, https://www.yuque.com/docs/share/cc33296f-5619-4ca2-a6bd-9f7e69adca11 
第十四期成长与晋升|密码:vmfn, https://www.yuque.com/docs/share/a0e478a2-bddf-47a8-98a9-2abd15f75815
第十三期搞构建|密码:gq54, https://www.yuque.com/docs/share/27d51f72-aa04-4244-82de-1e8fdd146581
第十二届可视化|密码:ax6w, https://www.yuque.com/docs/share/60bb15e6-a3a4-4e6c-8792-aabb7a175840
第十一届女生专场|密码:qbk3, https://www.yuque.com/docs/share/9a4d9535-db64-4a11-8989-231bc31afbfb
第十届跨端跨栈|密码:pq5m, https://www.yuque.com/docs/share/71044d2e-afd8-490f-b2b2-b3d1e4c2bd3d
第九届在线文档|密码:gevv, https://www.yuque.com/docs/share/00b31ead-3b71-4523-82e3-220c16f2a2c2
第八届面试攻略|密码:dd7a, https://www.yuque.com/docs/share/47e58557-b841-4d12-869f-086b8516b7d2
第七届微前端|密码:mgw9, https://www.yuque.com/docs/share/403a1c32-0677-4223-9190-15ac00fd01b1
第六届 Serverless|密码:xmxf, https://www.yuque.com/docs/share/e475386e-3c04-4b98-bb54-b64ab51dcc86
第五届前端监控|密码:vzut, https://www.yuque.com/docs/share/50c12699-b8c5-4e38-bbf2-e6380aa940c0
第四届职业规划|密码:sxy3, https://www.yuque.com/docs/share/3588f1be-d801-4830-993d-1ce976b148f8
第三届页面搭建|密码:ru81, https://www.yuque.com/docs/share/26e7e319-217d-4bc7-a2a6-3c70f87ff638
第二届前端基建|https://www.yuque.com/zaotalk/posts/s2ppt
第一届前端管理|https://www.yuque.com/zaotalk/posts/s1ppt



### 使用ECharts实现百分比条

```js
this.chart.setOption({
    grid: {
        width: '98%',
        left: 0,
        bottom: 10,
        containLabel: true
    },
    xAxis: {
        min: 0,
        max: 300,
        interval: 30,
        type: 'value',
        splitLine: {show: false},
        axisLabel: {
            show: true,
            margin: 40,
            interval: 0,
            showMaxLabel: true,
            formatter: function (value, index) {
                return `${value}%`;
            }
        },
        axisTick: {show: false},
        axisLine: {show: false}
    },
    yAxis: {
        type: 'category',
        axisTick: {show: false},
        axisLine: {show: false},
        axisLabel: {
            color: '#333',
            fontSize: 14
        },
        data: ['费效比']
    },
    series: [
        {
            name: '%',
            type: 'bar',
            barWidth: 20,
            // 设置百分比
            data: [percent],
            animationDuration: 1000,
            label: {
                show: true,
                position: 'right',
                offset: offset,
                color: '#333',
                fontSize: 14,
                // 自定义label
                formatter: function (params) {
                    return `{a|${params.value}%}\n{b|}`;
                },
                rich: {
                    a: {
                        color: '#333',
                        fontSize: 14,
                    },
                    b: {
                        width: 2,
                        height: 10,
                        fontSize: 14,
                        backgroundColor: '#1890FF',
                        borderRadius: 4,
                        align: 'center',
                    },
                }
            },
            itemStyle: {
                normal: {
                    barBorderRadius: radius,
                    color: '#1890FF'
                }
            },
            zlevel: 1
        },
        // 背景
        {
            name: '进度条背景',
            type: 'bar',
            barGap: '-100%',
            barWidth: 20,
            data: [300],
            color: '#D0E6F9',
            itemStyle: {
                normal: {
                    barBorderRadius: 10
                }
            },
            emphasis: {
                itemStyle: {
                    color: '#D0E6F9'
                }
            }
        }
    ]
});
```

### 前端数据分页

```js
/**
 * @name  getDataPagination
 * @desc  前端分页方法
 * @param  {Number} currentPage 当前页码，默认1
 * @param  {Number} pageSize 每页最多显示条数，默认10
 * @param  {Array} totalData 总的数据集，默认为空数组
 * @return {Object} {
    list, //当前页展示数据，数组
    currentPage, //当前页码
    pageSize, //每页最多显示条数
    total, //总的数据条数
 }
 **/
getDataPagination (totalData = [], currentPage = 1, pageSize = 10) {
    const { length } = totalData;
    const tableData = {
        list: [],
        currentPage,
        pageSize,
        total: length
    };
    // pageSize大于等于总数据长度，说明只有1页数据或没有数据 直接取第一页
    if (pageSize >= length) {
        tableData.list = totalData;
        tableData.currentPage = 1;
    } else { // pageSize小于总数据长度，数据多余1页
        // 计算当前页（不含）之前的所有数据总条数
        const num = pageSize * (currentPage - 1);
        // 如果当前页之前所有数据总条数小于（不能等于）总的数据集长度，则说明当前页码没有超出最大页码
        if (num < length) {
            // 当前页第一条数据在总数据集中的索引
            const startIndex = num;
            // 当前页最后一条数据索引
            const endIndex = num + pageSize - 1;
            // 当前页数据条数小于每页最大条数时，也按最大条数范围筛取数据
            tableData.list = totalData.filter((_, index) => index >= startIndex && index <= endIndex);
        } else { // 当前页码超出最大页码，则计算实际最后一页的page，自动返回最后一页数据
            // 取商
            const size = parseInt(length / pageSize);
            // 取余数
            const rest = length % pageSize;
            // 余数大于0，说明实际最后一页数据不足pageSize，应该取size+1为最后一条的页码
            if (rest > 0) {
                // 当前页码重置，取size+1
                tableData.currentPage = size + 1;
                tableData.list = totalData.filter((_, index) => index >= (pageSize * size) && index <= length);
            } else if (rest === 0) { // 余数等于0，最后一页数据条数正好是pageSize
                // 当前页码重置，取size
                tableData.currentPage = size;
                tableData.list = totalData.filter((_, index) => index >= (pageSize * (size - 1)) && index <= length);
            }
            // 注：余数不可能小于0
        }
    }
    return tableData;
}
```

