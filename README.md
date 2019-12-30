# carousel-slider

> A Vue CarouselSlider Component
* 基于Vue2.0，目前支持自动轮播，手势滑动，点击滑动，可配置自动滑动时间间隔，基本满足大部分功能

## Usage

``` bash

import { slider} from 'carousel-slider'

<slider :images="images"/>
```
## 支持的API

| 属性          | 说明                     | 默认 |
| ------------- | ------------------------ | ---- |
| autoPlay      | 是否自动轮播             | true |
| showIndex | 是否显示轮播的那个点     | true |
| duration      | 两次滚动之间间隔时间     | 3000 |
| images      | 轮播图素材     | [] |
