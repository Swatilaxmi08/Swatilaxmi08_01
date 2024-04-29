import sys
import datetime

def help():
    print("""
    Commands:
        add <todo>        - Add a new todo
        ls                - List all todos
        del <number>      - Delete a todo by number
        done <number>     - Mark a todo as done by number
        report            - Show todo statistics
        help              - Show this help message
    """)

def add(todo):
    with open('todos.txt', 'a') as f:
        f.write(f"{todo}\n")
    print(f"Added todo: {todo}")

def ls():
    try:
        with open('todos.txt', 'r') as f:
            todos = f.readlines()
        for i, todo in enumerate(todos, 1):
            print(f"[{i}] {todo.strip()}")
    except FileNotFoundError:
        print("No todos found.")

def del_todo(number):
    try:
        with open('todos.txt', 'r') as f:
            todos = f.readlines()
        with open('todos.txt', 'w') as f:
            for i, todo in enumerate(todos, 1):
                if i != int(number):
                    f.write(todo)
        print(f"Deleted todo #{number}")
    except (ValueError, IndexError):
        print("Error: Invalid todo number.")

def done(number):
    try:
        with open('todos.txt', 'r') as f:
            todos = f.readlines()
        with open('todos.txt', 'w') as f:
            for i, todo in enumerate(todos, 1):
                if i == int(number):
                    with open('done.txt', 'a') as done_f:
                        done_f.write(f"{datetime.datetime.now()} {todo.strip()}\n")
                else:
                    f.write(todo)
        print(f"Marked todo #{number} as done.")
    except (ValueError, IndexError):
        print("Error: Invalid todo number.")

def report():
    try:
        with open('todos.txt', 'r') as f:
            todos = f.readlines()
        with open('done.txt', 'r') as done_f:
            dones = done_f.readlines()
        print(f"{len(todos)} pending todos, {len(dones)} done.")
    except FileNotFoundError:
        print("No todos found.")

if __name__ == '__main__':
    if len(sys.argv) < 2:
        help()
        sys.exit(0)
    command = sys.argv[1]
    if command == 'help':
        help()
    elif command == 'add':
        add(sys.argv[2])
    elif command == 'ls':
        ls()
    elif command == 'del':
        del_todo(sys.argv[2])
    elif command == 'done':
        done(sys.argv[2])
    elif command == 'report':
        report()
    else:
        print("Error: Invalid command.")
