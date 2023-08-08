# Writting docs

After introducing the basic principles of building this website and implementing automated deployment, writing a document is very easy. This article briefly introduces how to contribute documents.

We will not introduce the basic syntax of markdown, but we will briefly introduce pymdown's extension of markdown syntax, and introduce the standard we write markdown documents.

## Code blocks

We use backquotes to insert a code block in markdown:

````markdown
Some text
``` language-you-use
Some code
```
Some text
````

But not this:

```markdown
Some text

    Some code
    Some code

Some text
```

This syntax disables syntax highlighting, and makes it more difficult to read the code and adding annotations. If you want to insert \`\`\` in your code block (like this document), just use more backquotes:

`````markdown
Some text
````markdown
Some markdown code
```language-you-use
Some code
```
````
`````

## Using Admonitions

Use admonitions the Material for MkDocs provided, this will make your output much prettier and focused on the content.

!!! tip inline end 
    
    Use admonitions properly, or you will get your documentation more confusing.

Guide using admonitions see [this](https://squidfunk.github.io/mkdocs-material/reference/admonitions/) link in Material for MkDocs documentation.

### Using inline admonitions

Inline admonitions provided a simple way to render admonition without breaking documentation into parts. This function is useful when writing a tutorial or "cookbook". When the viewport is too narrow to put the inline admonitions, it will stretch to the full width of the viewport, one case is reading documentation on mobile phones.

!!! info inline end "Lorem ipsum dolor"
    
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo purus auctor massa, nec semper lorem quam in massa.

## Using annotations

Annotations is one of the best features of the Material for MkDocs I considered. It provided another simple way to make comment-like notes in almost anywhere in markdown document. 

For it's usage, click [this](https://squidfunk.github.io/mkdocs-material/reference/annotations/) to learn more.

Here is a code block which commented by annotations. It is a thread-safe log queue interface written in C++. Notice we also used `linenums`, `title` and `hl_lines` attributes.

Result looks like this:

```cpp title="log_queue.hpp" linenums="1" hl_lines="27-28"
class LogQueue
{
    private:
        std::queue<std::string, std::list<std::string>> message_queue; // (1)!
        std::mutex queue_mutex;
        std::condition_variable queue_cond;
        
        std::thread internal_thread;

        bool exit_flag {false};
        std::mutex ef_mutex;
        
        std::ostream &stream; // (2)!

        void listen(void);
    public:
        LogQueue(void) : 
            stream {std::cout},
            internal_thread {&LogQueue::listen}
        { }

        LogQueue(LogQueue const&) = delete;
        LogQueue &operator=(LogQueue const&) = delete;

        virtual ~LogQueue(void);

        void add(std::string);
        void stop(void); // (3)!
};
```

1. The message queue wraps the `std::list`, which may increase the performance.

2. Notice that `stream` should always initialized in ctor-initializer list. Don't place this in scope, that occurs compile error.

3. This function uses the `ef_mutex`, which protects the `exit_flag` variable, `internal_thread` checks the flag, when it was set, loop in thread exits.


The raw code looks like this:

````markdown
```cpp title="log_queue.hpp" linenums="1" hl_lines="27-28"
class LogQueue
{
    private:
        std::queue<std::string, std::list<std::string>> message_queue; // (1)!
        std::mutex queue_mutex;
        std::condition_variable queue_cond;
        
        std::thread internal_thread;

        bool exit_flag {false};
        std::mutex ef_mutex;
        
        std::ostream &stream; // (2)!

        void listen(void);
    public:
        LogQueue(void) : 
            stream {std::cout},
            internal_thread {&LogQueue::listen}
        { }

        LogQueue(LogQueue const&) = delete;
        LogQueue &operator=(LogQueue const&) = delete;

        virtual ~LogQueue(void);

        void add(std::string);
        void stop(void); // (3)!
};
```

1. The message queue wraps the `std::list`, which may increase the performance.

2. Notice that `stream` should always initialized in ctor-initializer list. Don't place this in scope, that occurs compile error.

3. This function uses the `ef_mutex`, which protects the `exit_flag` variable, `internal_thread` checks the flag, when it was set, loop in thread exits.
````


