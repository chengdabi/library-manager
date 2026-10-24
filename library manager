#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <iomanip>

using namespace std;

#define FILENAME "books.txt"

/* 书的结构 */
struct Book {
    int    id;
    string title;
    string author;
    double price;
    int    stock;
};

/* 书的链表节点 */
struct booknode {
    Book     book;
    booknode* next;
};

/* 打印一本书 */
void printbook(const Book& b) {
    cout << "[" << b.id << "] 《" << b.title << "》 作者：" << b.author
         << " 价格：" << fixed << setprecision(2) << b.price
         << " 库存：" << b.stock << "\n";
}

/* 图书馆类 */
class library {
private:
    booknode* head = nullptr;
    int       count = 0;

    // 按编号查找节点，找不到返回 nullptr
    booknode* findbooknode(int id) {
        for (booknode* p = head; p != nullptr; p = p->next) {
            if (p->book.id == id) return p;
        }
        return nullptr;
    }

    // 在链表末尾加入一个节点
    void endnode(const Book& b) {
        booknode* node = new booknode;
        node->book = b;
        node->next = nullptr;
        if (head == nullptr) {
            head = node;
        } else {
            booknode* p = head;
            while (p->next != nullptr) p = p->next;
            p->next = node;
        }
        count++;
    }

    // 释放整条链表
    void clear() {
        booknode* p = head;
        while (p != nullptr) {
            booknode* next = p->next;   // 先保存下一个，再释放当前
            delete p;
            p = next;
        }
        head = nullptr;
        count = 0;
    }

public:
    // 析构函数：对象销毁时自动释放链表
    ~library() { clear(); }

    // 添加图书
    void addbook() {
        Book b;
        cout << "图书编号：";
        cin >> b.id;
        cin.ignore();
        if (findbooknode(b.id) != nullptr) {
            cout << b.id << " 已经存在，添加失败\n";
            return;
        }
        cout << "书名：";
        getline(cin, b.title);
        cout << "作者：";
        getline(cin, b.author);
        cout << "价格：";
        cin >> b.price;
        cin.ignore();
        cout << "库存：";
        cin >> b.stock;
        cin.ignore();
        endnode(b);
        cout << "添加成功，现在共 " << count << " 本书\n";
    }

    // 删除图书
    void remove() {
        int id;
        cout << "请输入要删除的图书编号：";
        cin >> id;
        cin.ignore();

        booknode* cur = head;
        booknode* pre = nullptr;
        while (cur != nullptr && cur->book.id != id) {
            pre = cur;
            cur = cur->next;
        }
        if (cur == nullptr) {          // 没找到
            cout << "未找到编号 " << id << " 的图书\n";
            return;
        }
        if (pre == nullptr) {          // 删的是头节点
            head = cur->next;
        } else {
            pre->next = cur->next;     // 前驱跳过 cur
        }
        delete cur;
        count--;
        cout << "删除成功\n";
    }

    // 查询图书
    void searchbook() {
        int op;
        cout << "查询方式：1=按编号  2=按书名\n请选择：";
        cin >> op;
        cin.ignore();
        if (op == 1) {
            int id;
            cout << "请输入编号：";
            cin >> id;
            cin.ignore();
            booknode* p = findbooknode(id);
            if (p != nullptr) printbook(p->book);
            else cout << "未找到编号 " << id << " 的图书\n";
        } else {
            string title;
            cout << "请输入书名：";
            getline(cin, title);
            bool found = false;
            for (booknode* p = head; p != nullptr; p = p->next) {
                if (p->book.title.find(title) != string::npos) {
                    printbook(p->book);
                    found = true;
                }
            }
            if (!found) cout << "未找到书名含 " << title << " 的图书\n";
        }
    }

    // 显示全部
    void appear() {
        if (head == nullptr) {
            cout << "暂无图书\n";
            return;
        }
        cout << "共 " << count << " 本：\n";
        for (booknode* p = head; p != nullptr; p = p->next) {
            printbook(p->book);
        }
    }

    // 按编号升序排序（冒泡：交换节点里的数据）
    void idsort() {
        for (booknode* i = head; i != nullptr && i->next != nullptr; i = i->next) {
            for (booknode* j = i->next; j != nullptr; j = j->next) {
                if (i->book.id > j->book.id) {
                    Book tmp = i->book;
                    i->book = j->book;
                    j->book = tmp;
                }
            }
        }
        cout << "已按编号升序排序\n";
    }

    // 保存到文本文件
    bool save(const string& filename) const {
        ofstream out(filename);
        if (!out) return false;
        for (booknode* p = head; p != nullptr; p = p->next) {
            out << p->book.id << "|" << p->book.title << "|" << p->book.author
                << "|" << p->book.price << "|" << p->book.stock << "\n";
        }
        return true;
    }

    // 从文本文件读取
    bool read(const string& filename) {
        ifstream in(filename);
        if (!in) return false;
        clear();                       // 先清空现有链表
        string line;
        while (getline(in, line)) {
            if (line.empty()) continue;
            stringstream ss(line);
            Book b;
            string idstr, pricestr, stockstr;
            getline(ss, idstr,    '|');
            getline(ss, b.title,  '|');
            getline(ss, b.author, '|');
            getline(ss, pricestr, '|');
            getline(ss, stockstr, '|');
            b.id    = stoi(idstr);
            b.price = stod(pricestr);
            b.stock = stoi(stockstr);
            endnode(b);
        }
        return true;
    }
};

/* 菜单 */
void menu() {
    cout << "=========================================\n";
    cout << "          图书管理系统\n";
    cout << "=========================================\n";
    cout << "  1. 添加图书       2. 删除图书\n";
    cout << "  3. 查询图书       4. 显示全部\n";
    cout << "  5. 按编号排序     6. 保存到文档\n";
    cout << "  7. 从文件读取\n";
    cout << "  0. 退出（自动保存）\n";
    cout << "-----------------------------------------\n";
}

int main() {
    library lib;
    if (lib.read(FILENAME)) {
        cout << "已从 " << FILENAME << " 载入数据。\n\n";
    } else {
        cout << "未找到数据文件，从空库开始。\n\n";
    }

    int running = 1;
    while (running) {
        menu();
        int choice;
        cout << "请选择：";
        cin >> choice;
        cin.ignore();

        switch (choice) {
            case 1: lib.addbook();    break;
            case 2: lib.remove();     break;
            case 3: lib.searchbook(); break;
            case 4: lib.appear();     break;
            case 5: lib.idsort();     break;
            case 6:
                if (lib.save(FILENAME)) cout << "  保存成功！\n";
                else cout << "  保存失败！\n";
                break;
            case 7:
                if (lib.read(FILENAME)) cout << "  读取成功！\n";
                else cout << "  读取失败！\n";
                break;
            case 0:
                if (lib.save(FILENAME)) cout << "  已保存数据，再见！\n";
                running = 0;
                break;
            default:
                cout << "  无效选项，请重新输入。\n";
        }
        cout << "\n";
    }
    return 0;
}
