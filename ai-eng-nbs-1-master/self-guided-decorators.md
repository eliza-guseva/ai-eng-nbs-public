q   # Self-Guided Journey into Logging and Python Decorators

Welcome! This guide will help you discover decorators at your own pace. Each section has:
- 🎯 A clear goal
- 📝 Step-by-step explanation
- ✅ Self-check code to verify you got it right
- 🤔 Understanding checks
- 🎨 Creative exercises

# Introduction (Why we are doing this)

Every software engineer, AI or not, eventually has that moment when their code fails and they have no idea why. They add print statements everywhere, run it again, wait... slowly the bug is found, the print statements are removed, and then the next day another bug makes them to start the process again. In order to make this process more organized and stable and also to know what exactly is going on in the code we use proper logging.

Logging is like having security cameras in your code - they record what's happening even when you're not actively watching. But adding logging code everywhere can make your actual business logic messy and hard to read.

This is where decorators come in. Instead of adding logging code inside every function, we can write it once as a decorator and then just tag the functions we want to monitor. It's like installing a security system once and then just putting sensors where you need them, rather than building custom surveillance into every room.

## Level 1: Functions are Just Objects! 
🎯 Goal: Understand that functions in Python are objects we can pass around

### Step 1: Functions as Variables
```python
def say_hello(name):
    return f"Hello, {name}!"

# Try this:
greeting_function = say_hello  # Note, we don't add parentheses, it's because we are not calling the function, we just creating another variable that points to our original function
result = greeting_function("Alice")
print(result)  # Should print: Hello, Alice!
```

🤔 Understanding Check:
```python
# Copy and run this code - it will tell you if you understand:
def self_check_1():
    def double(x):
        return x * 2
    
    # Your task: Create a new variable that points to the double function
    # Then use it to double the number 5
    ###!!! Your code here !!!###
    
    # Self-check
    try:
        if result == 10 and doubler is double:
            print("🎉 You got it! You understand functions as objects!")
        else:
            print("🤔 Not quite. Remember: don't use parentheses when assigning the function")
    except NameError:
        print("Please write the code where it's indicated")

self_check_1()
```

### Step 2: Storing values inside functions

Functions in Python can be created with values permanently stored inside them. This is crucial for decorators - when we write a logging decorator, it needs to keep track of which function it's wrapping. Let's start with a simpler example where we store numbers instead of functions:

```python
def create_multiplier(factor):
    def multiplier(x):
        return x * factor
    return multiplier

# Try this:
double = create_multiplier(2) # storing multiplier 2
triple = create_multiplier(3) # storing multiplier 3

print(double(5))  # Should print: 10
print(triple(5))  # Should print: 15 
```

🤔 Understanding Check:
```python
def self_check_2():
    # Your task:
    # 1. Create a function create_greeter that takes a greeting word 
    #    and returns a function that greets people with that word
    # 2. Use your create_greeter to make casual and formal greeters
    #    2.1 Create a casual greeter that says "Hi, <name>!"
    #    2.2 Create a formal greeter that says "Good morning, <name>!"
    # 3. Put both greetings to Bob in a list called 'results'
    ###!!! Your code here !!!###
    results = []
    
    expected = ["Hi, Bob!", "Good morning, Bob!"]
    
    if results == expected:
        print("🎉 Perfect! You understand nested functions!")
    else:
        print("🤔 Not quite. Check how create_greeter is used in the example")

self_check_2()
```

## Level 2: Your First Decorator
🎯 Goal: Write your first decorator and understand how it wraps functions

### Step 1: Building a Function That Uses Another Function
So, we know how to store numbers inside functions. Now let's store a function inside another function and use it:
```python
def create_logger(func):
    def wrapper():
        print("Starting!")  # extra step before
        func()             # using the stored function
        print("Done!")     # extra step after
    return wrapper

# Try this:
def say_hello():
    print("Hello!")

logged_hello = create_logger(say_hello)  # stores say_hello inside wrapper
logged_hello()  # Should print:
                # Starting!
                # Hello!
                # Done!

@create_logger       # same as logged_hello = create_logger(say_hello)
def say_hello():
    print("Hello!")

say_hello()         # same result as running logged_hello above
```

🤔 Understanding Check:
```python
def self_check_3():
    # Your task:
    # 1. Create a function add_stars that takes a function
    #    and returns a new function that:
    #    - prints "***" before running the stored function
    #    - runs the stored function
    #    - prints "***" after running the stored function
    # 2. Use @ to add stars to hello_world function
    ###!!! Your code here !!!###
    
    @add_stars
    def hello_world():
        print("Hello World!")
    
    # Self-check (checking print output)
    import io
    import sys
    captured_output = io.StringIO()
    sys.stdout = captured_output
    
    hello_world()
    sys.stdout = sys.__stdout__
    
    expected_output = "***\nHello World!\n***\n"
    if captured_output.getvalue() == expected_output:
        print("🎉 You've created your first decorator!")
    else:
        print("🤔 Check your output. Should be exactly: ***, Hello World!, ***")

self_check_3()
```

## Level 3: Building a Real Logger
🎯 Goal: Create a decorator that helps us track what our functions are doing

Remember how print statements help us debug? This time, let's make them actually useful. Every function in Python has a special property __name__ that tells us what the function is called. So if we create a function process_payment(), Python remembers that name and we can access it using process_payment.__name__. This is super useful for logging - instead of hardcoding function names, we can automatically get them!

We also add timestamps to our logs - when debugging issues in real applications, knowing exactly when each function started and finished is crucial. If something went wrong at 2:45 PM, we can look at all function calls around that time. Or if a function is taking too long, we can see exactly how long by comparing start and end times.
Here's how we can use these ideas to build a real logging system:

```python
import time
from datetime import datetime

def log_activity(func):
    def wrapper(*args, **kwargs):
        timestamp = datetime.now().strftime('%H:%M:%S')
        print(f"[{timestamp}] Starting {func.__name__}")
        result = func(*args, **kwargs)
        print(f"[{timestamp}] Completed {func.__name__}")
        return result
    return wrapper

# Try this:
@log_activity
def process_payment():
    time.sleep(1)  # Simulate some work
    return "Payment processed"

print(process_payment())  # Should print:
# [14:23:45] Starting process_payment
# [14:23:46] Completed process_payment
# Payment processed
```

You might have noticed *args and **kwargs in our wrapper function. These are Python's way of saying "accept any arguments that come in." We need this because we want our decorator to work with any function, no matter what arguments it takes.
Let's break it down:

`*args` captures all regular arguments as a tuple. When someone calls `test_division(6, 2)`, args becomes (6, 2)
`**kwargs` captures all keyword arguments as a dictionary. If someone `calls test_division(a=6, b=2)`, kwargs becomes `{"a": 6, "b": 2}`

Then `func(*args, **kwargs)` passes these arguments back to the original function exactly as they came in. It's like having a mail forwarding service - whatever mail comes in, we send it along unchanged.

Without this, our decorator would only work with functions that take no arguments! Try removing `*args, **kwargs` from the example above and see what happens when you try to use division with two numbers.

🤔 Understanding Check:
```python
def self_check_4():
    # Your task:
    # 1. Create a decorator error_logger that:
    #    - Prints "[ERROR]" if function raises an error
    #    - Prints "[SUCCESS]" if function completes
    #    - Always includes function name in the message
    # 2. Use it on test_division below
    ###!!! Your code here !!!###
    
    @error_logger
    def test_division(a, b):
        return a / b
    
    # Let's test both success and failure
    test_division(6, 2)  # Should print: [SUCCESS] test_division
    try:
        test_division(6, 0)  # Should print: [ERROR] test_division
    except:
        pass

    # Self-check
    if test_division.success_count == 1 and test_division.error_count == 1:
        print("🎉 Your error logger works correctly!")
    else:
        print("🤔 Make sure you're tracking both successes and errors")

self_check_4()
```

## Creative Exercises
Now that you understand the basics, try these exercises:

1. Create a `@debug` decorator that prints:
   - Function name
   - Arguments passed
   - Return value
   ```python
   @debug
   def add(a, b):
       return a + b
   
   add(3, 4)
   # Should print:
   # Calling add with args: (3, 4)
   # add returned: 7
   ```

2. Create a `@retry` decorator that retries a function if it fails:
   ```python
   @retry(max_attempts=3)
   def might_fail():
       import random
       if random.random() < 0.7:
           raise ValueError("Bad luck!")
       return "Success!"
   ```


## Verifying Your Solutions

For each creative exercise, here's how to know if you got it right:

### Debug Decorator Check
```python
def check_debug_decorator():
    import io
    import sys
    
    # Capture print output
    captured_output = io.StringIO()
    sys.stdout = captured_output
    
    @debug
    def test_func(x, y):
        return x + y
    
    result = test_func(5, 3)
    
    # Restore stdout
    sys.stdout = sys.__stdout__
    
    output = captured_output.getvalue()
    if ("test_func" in output and 
        "5" in output and 
        "3" in output and 
        "8" in output):
        print("🎉 Your debug decorator works perfectly!")
    else:
        print("🤔 Make sure you're printing function name, args, and return value")

check_debug_decorator()
```

### Retry Decorator Check
```python
def check_retry_decorator():
    attempts = 0
    
    @retry(max_attempts=3)
    def test_func():
        nonlocal attempts
        attempts += 1
        raise ValueError("Testing retry")
    
    try:
        test_func()
    except ValueError:
        if attempts == 3:
            print("🎉 Your retry decorator made exactly 3 attempts!")
        else:
            print(f"🤔 Expected 3 attempts, but got {attempts}")

check_retry_decorator()
```


## Need Help?

If you get stuck:
1. Re-read the previous sections
2. Run the self-check code and read the error messages carefully
3. Try breaking down the problem into smaller pieces
4. Remember: every decorator is just a function that takes a function and returns a new function

You've got this! 💪


# Solutions

## Error logger
```python
def error_logger(func):
    def wrapper(*args, **kwargs):
        try:
            result = func(*args, **kwargs)
            print(f"[SUCCESS] {func.__name__}")
            wrapper.success_count += 1
            return result
        except:
            print(f"[ERROR] {func.__name__}")
            wrapper.error_count += 1
            raise
    
    # Initialize counters
    wrapper.success_count = 0
    wrapper.error_count = 0
    return wrapper
```
