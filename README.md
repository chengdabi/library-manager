# library-manager
# 图书管理系统（C++ · 链表）

软件组后端面试加分小项目。用 C++ 实现一个命令行图书管理系统，基于**链表 + 面向对象**，支持文件持久化。

## 功能

- 添加 / 删除 / 修改图书
- 查询（按编号精确查、按书名部分匹配）
- 显示全部、按编号升序排序
- 保存到文件 / 从文件读取（重启不丢数据）
- 退出时自动保存

## 编译运行

需要支持 C++17 的编译器（g++ / MinGW / Dev-C++ 均可）。

```bash
g++ -std=c++17 library_manager.cpp -o library_manager
./library_manager      # Windows 下为 library_manager.exe
```

## 技术点

- **链表**：节点用 `next` 指针串起来，可随时增删
- **动态内存**：`new` / `delete`，析构函数自动释放、不内存泄漏
- **面向对象**：`class library` 把数据与操作封装在一起
- **文件读写**：`ofstream` / `ifstream` 文本持久化
- `string`、`getline`、`cin.ignore()`

## 结构

| 部分 | 说明 |
| --- | --- |
| `struct Book` | 一本书的数据（编号/书名/作者/价格/库存） |
| `struct booknode` | 链表节点 |
| `class library` | 增删改查、排序、文件读写 |

## 作者

成梓聪
