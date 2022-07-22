# react-dagre-map-v2

## Getting Started

Install dependencies,

```bash
$ npm i react-dagre-map-v2
```

联系/contact
``` 
 1047664386@qq.com
```

TodoList

```
 1.Documentation will be improved in the future /补充文档
 2.Optimize code to improve performance /优化代码，处理bug
 3.Add animation /增加动画
 4.Add Vue version
```
Demo:

```tsx
import React from 'react';
import { DagreMap } from 'react-dagre-map-v2';

interface ICard {
  readonly id: number | string;
  up?: ICard[];
  down?: ICard[];
  upLength: number;
  downLength: number;
  [propName: string]: any;
}

interface INode extends ICard {
  uuid: number;
  rank: number;
  rootUUID: number;
  parentID: number | null;
  visible?: boolean;
  hideUp?: boolean;
  hideDown?: boolean;
  [propName: string]: any;
}

// 样式参数设置
enum DEFAULT_STYLE {
  OFFSET_L = 40, // 整体距离页面左边的初始间距。
  OFFSET_R = 40, // 整体距离页面右边的初始间距。
  OFFSET_TOP = 20, // 图层组元素的初始纵向间距
  NODE_OFFSET_Y = 30, // 每个图层组元素的纵向间距
  NODE_OFFSET_X = 100, // 元素左右之间间距 100，
  CR = 8, // 收缩展开按钮圆形半径，是元素之外的位置 ，同时距离元素margin也是一个半径.
  NODE_WIDTH = 340, // 元素本身宽度，
  NODE_HEIGHT = 124, // 元素本身高度，
}

// 节点朝向， root 根节点 up上节点 down下节点
enum ARROW_DIR {
  UP = -1,
  DOWN = 1,
  ROOT = 0,
}

type TArrow = ARROW_DIR.UP | ARROW_DIR.DOWN | ARROW_DIR.ROOT;

interface IFieldNames {
  id: string;
  up: string;
  down: string;
  upLength: string;
  downLength: string;
}

export default () => {
  let data: ICard[] = [
    {
        id:1,
        name:'i am root one',
        upLength:2,
        up:[
            {
                id:3,
                name:'i am his up children one',
                upLength:2,
                up:[] //when up.length !== upLength,show +,can use fetchMoreData
            },
            {
                id:4,
                name:'i am his up children two',
                upLength:0,
                up:[]
            }
        ],
        downLength:1,
        down:[
            {
                id:5,
                name:'i am his down children one',
                upLength:0,
                downLength:0,
            },
        ]
    },
    {
        id:2,
        name:'i am root two',
        upLength:0,
        downLength:0,
    },
  ]

  return (
    <DagreMap
      renderChild={(info: INode) => {
        return <div>{info.uuid}我是自己diy的子节点样式</div>;
      }}
      data={dataList}
      fetchMoreData={(info: INode, direction: TArrow): Promise<{data: ICard[]}> => {
        // 异步返回下一级节点 {data:[],code:200,msg}
        return new Promise((res, rej) => {
          res({ data: [{ id: 1000, name: '' }] });
        });
      }}
      offset={{
        left = DEFAULT_STYLE.OFFSET_L,
        top = DEFAULT_STYLE.OFFSET_TOP,
        right = DEFAULT_STYLE.OFFSET_R,
      }}
      nodeStyle={{
        offsetY = DEFAULT_STYLE.NODE_OFFSET_Y,
        offsetX = DEFAULT_STYLE.NODE_OFFSET_X,
        width = DEFAULT_STYLE.NODE_WIDTH,
        height = DEFAULT_STYLE.NODE_HEIGHT,
        circleWidth = DEFAULT_STYLE.CR,
      }}
      fieldNames={{
        id: 'id', // id,用来request,
        up: 'up', // root节点的up子节点 === left
        down: 'down', // root节点的down子节点 === right
        upLength: 'upLength', // 上子节点个数，控制是否可以新增
        downLength: 'downLength', //下子节点个数，控制是否可以新增
      }}
    />
  );
};
```
