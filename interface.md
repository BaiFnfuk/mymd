## 接口

### 串行通讯的分类

- 按通讯方式

  - 同步通讯：带时钟同步信号传输。比如：SPI（全双工），IIC（半双工）通信接口。
  - 异步通讯：不带时钟同步信号。比如：UART（全双工），单总线（半双工）。

- 按数据传送方向

  - 单工：只支持数据在一个方向上传输；
  - 半双工：允许数据在两个方向上传输，某一时刻只有一个传输方向；
  - 全双工：允许数据同时在两个方向上传输。

  

### uart

- 异步串行，全双工
- tx, rx, gnd



### usart

- 同步&异步,全双工

- tx,rx,gnd

### spi

- 同步串行，全双工

- sclk(时钟),  sdi（数据输入）,  sdo（数据输出）, cs（片选）



### i2c

- 同步，半双工
- scl, sda