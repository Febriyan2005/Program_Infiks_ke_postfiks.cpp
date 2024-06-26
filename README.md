# Program_Infiks_ke_postfiks.cpp
Praktek cpp minggu ke 11 Tentang Infiks ke postfiks.

#include <iostream>
#include <stack>
#include <string>
#include <cctype>

// Fungsi untuk memeriksa apakah karakter adalah operator
bool isOperator(char c) {
    return (c == '+' || c == '-' || c == '*' || c == '/' || c == '^');
}

// Fungsi untuk memeriksa prioritas operator
int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    if (op == '^') return 3;
    return 0;
}

// Fungsi untuk melakukan konversi dari infiks ke postfiks
std::string infixToPostfix(const std::string& infix) {
    std::stack<char> operators;
    std::string postfix;

    for (char c : infix) {
        if (std::isspace(c)) {
            continue;
        } else if (std::isdigit(c) || std::isalpha(c)) {
            postfix += c;
        } else if (c == '(') {
            operators.push(c);
        } else if (c == ')') {
            while (!operators.empty() && operators.top() != '(') {
                postfix += operators.top();
                operators.pop();
            }
            if (!operators.empty()) operators.pop();
        } else if (isOperator(c)) {
            while (!operators.empty() && precedence(operators.top()) >= precedence(c)) {
                postfix += operators.top();
                operators.pop();
            }
            operators.push(c);
        }
    }

    while (!operators.empty()) {
        postfix += operators.top();
        operators.pop();
    }

    return postfix;
}

int main() {
    std::string infix;
    std::cout << "Masukkan ekspresi infiks: ";
    std::getline(std::cin, infix);

    std::string postfix = infixToPostfix(infix);
    std::cout << "Ekspresi postfiks: " << postfix << std::endl;

    return 0;
}
