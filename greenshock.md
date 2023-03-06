greenshock

## Tween：单个动画

- gasp.to()：设置 animate 终点
- gasp.from()：设置 animate 起点
- gasp.formTo()：设置 animate 起点和终点

## variable

- ### ease (important)

  用于控制动画变化速率

  `'power1.out'` 是最常用的，因为它响应很快但是结束很慢，给人舒适的感觉

- ### duration

  动画持续时间

- ### delay

  动画将会延迟 delay 秒

- ### repeat

  动画重复次数

- ### yoyo

  搭配 `repeat` 使用，每次重复的时候，Tween 会往相反的方向进行

- ### stagger

  如果提供多个目标，会在每个目标的动画之前添加间隔

## timeline：管理多个动画

