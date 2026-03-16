---
_elasticsearch_data_sharing_indexed_on: "2024-11-18 13:55:30"
_publicize_job_id: "44783992242"
author: gmirchev90
categories:
  - desktop
  - web
date: "2020-05-28T06:16:09+00:00"
guid: http://georgimirchev.com/?p=269
parent_post_id: null
post_id: "269"
summary: I am using [axios](https://github.com/axios/axios) to scrape some data from some websites. Currently, I hit the case where the encoding of the website is in windows-1251 and the response I got from the request wasn't readable at all.
tags:
  - axios
  - cyrillic
  - nodejs
  - utf8
  - windows1251
timeline_notification: "1590646572"
title: Parse windows-1251 (cyrillic) request in axios
url: /2020/05/28/parse-windows-1251-cyrillic-request-in-axios/

---
I am using [axios](https://github.com/axios/axios) to scrape some data from some websites. Currently, I hit the case where the encoding of the website is in windows-1251 and the response I got from the request wasn't readable at all.

Here is a simple way to fix it:

```
axiosResponseTransformer: AxiosTransformer = (data: any, headers?: any) => {
        const iconv = new Iconv("windows-1251", "utf-8");
        data = iconv.convert(data);
        return data
    };

    axiosConfig: AxiosRequestConfig = {
        responseType: "arraybuffer",
        transformResponse: this.axiosResponseTransformer
    };
    client = axios.create(this.axiosConfig);
```

Just install the [iconv](https://www.npmjs.com/package/iconv) package and then register a transformer that will convert the data from binary ("arraybuffer") to data with utf-8 encoding. :)
