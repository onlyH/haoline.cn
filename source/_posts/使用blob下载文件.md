---
title: 使用blob下载文件
date: 2021-07-05 10:28:23
tags:
---
<blockquote class="blockquote-center">
使用 bolb 下载文件并修改文件名称
</blockquote>
<!-- more -->


```javascript
 getDownload(url: string, requestData: any) {
        // 响应类型：arraybuffer, blob
        axiosservice
            .get(url, {params: requestData, responseType: 'blob'})
            .then((resp) => {
                const headers = resp.headers;
                const contentType = headers["content-type"];

                console.log("响应头信息", headers);
                if (!resp.data) {
                    console.error("响应异常：", resp);
                    return false;
                } else {
                    console.log("下载文件：", resp);
                    const blob = new Blob([resp.data], {type: contentType});

                    const contentDisposition = resp.headers["content-disposition"];
                    let fileName = "unknown";
                    if (contentDisposition) {
                        fileName = window.decodeURI(resp.headers["content-disposition"].split("=")[1]);
                    }
                    let name = null
                    if (requestData.fileName) {
                        name = fileName.split('.')
                        fileName = name[0] + '(' + requestData.fileName + ')' + '.' + name[1]
                        downFile(blob, fileName);
                    } else {
                        downFile(blob, fileName);
                    }

                }
            })
            .catch(function (error) {
                console.log(error);
            });
    }
```

```javascript
/* 下载方法 */
function downFile(blob: any, fileName: string) {
    // 非IE下载
    if ("download" in document.createElement("a")) {
        const link = document.createElement("a");
        link.href = window.URL.createObjectURL(blob); // 创建下载的链接
        link.download = fileName; // 下载后文件名
        link.style.display = "none";
        document.body.appendChild(link);
        link.click(); // 点击下载
        window.URL.revokeObjectURL(link.href); // 释放掉blob对象
        document.body.removeChild(link); // 下载完成移除元素
    } else {
        // IE10+下载
        window.navigator.msSaveBlob(blob, fileName);
    }
}
```

```javascript
// 使用：

// 下载模板
handleClickDownload() {
    let Label = this.$refs["deviceCascader"].getCheckedNodes()[0].pathLabels
    let fileName = Label[Label.length - 1]
    const getParams = {
        projectId: this.projectId,
        classCode: this.aliasCode,
        stage: 'dpm',
        fileName
    }
    getDownload(getParams);
}

export function getDownload(postParams: any): any {
    return httputils.postJson(`${baseApi}/object/download`, postParams)
}


```
