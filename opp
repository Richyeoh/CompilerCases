use std::collections::HashMap;

fn infix_to_postfix(expr: &str) -> String {
    let mut postfix = String::new();
    
    let mut stack = Vec::new();
    
    let mut precedence = HashMap::new();

    precedence.insert('(', 0);
    precedence.insert(')', 0);
    precedence.insert('+', 1);
    precedence.insert('-', 1);
    precedence.insert('*', 2);
    precedence.insert('/', 2);

    for c in expr.chars() {
        match c {
            '0'..='9' => postfix.push(c), // 操作数直接输出到后缀表达式
            '(' => stack.push(c), // 左括号入栈
            ')' => {
                // 弹出栈中的运算符直到遇到左括号为止
                while let Some(top) = stack.pop() {
                    if top == '(' {
                        break;
                    }
                    postfix.push(top);
                }
            }
            '+' | '-' | '*' | '/' => {
                // 当前运算符的优先级大于栈顶运算符的优先级时，将当前运算符入栈
                while let Some(&top) = stack.last() {
                    if precedence[&c] > precedence[&top] {
                        break;
                    }
                    postfix.push(stack.pop().unwrap());
                }
                stack.push(c);
            }
            _ => {}
        }
    }

    // 将栈中剩余的运算符依次弹出并加入到后缀表达式中
    while let Some(op) = stack.pop() {
        postfix.push(op);
    }

    postfix
}

fn eval_postfix(expr: &str) -> Option<f64> {
    let mut stack = Vec::new();

    // 3 12 5

    for token in expr.chars() {
        if token.is_digit(10) {
            stack.push(token.to_digit(10).unwrap() as f64); // 操作数入栈
        } else {
            let b = stack.pop()?; // 弹出栈顶的两个操作数
            let a = stack.pop()?;
            let result = match token {
                '+' => a + b,
                '-' => a - b,
                '*' => a * b,
                '/' => a / b,
                _ => return None, // 非法运算符返回 None
            };
            stack.push(result); // 计算结果入栈
        }
    }

    stack.pop() // 返回栈顶的结果
}

fn main() {
    let infix_expr = "1+2-3*4/5";
    let postfix_expr = infix_to_postfix(infix_expr);
    println!("Infix: {}", infix_expr);
    println!("Postfix: {}", postfix_expr);

    if let Some(result) = eval_postfix(&postfix_expr) {
        println!("Result: {}", result);
    } else {
        println!("Invalid expression");
    }
}
