## FunctionDef now
**now**: 此函数的功能是获取当前的本地时间，并以特定的格式返回。

**参数**: 此函数没有参数。

**代码描述**: `now` 函数使用了Python的`time`模块来获取当前的本地时间，并通过`strftime`方法将时间格式化为“年月日时分秒”的形式（即"%Y%m%d%H%M%S"）。这种时间格式便于进行日志记录、文件命名等操作，因为它能够以一种易于理解且不容易引起混淆的方式表示时间点。在`chatdev`项目中，`now`函数被多个地方调用，主要用于生成日志文件的路径和记录软件的开始与结束时间。

例如，在`chatdev/chat_chain.py`的`get_logfilepath`方法中，`now`函数用于获取软件开始运行的时间，这个时间戳被用来构造日志文件的名称，确保每个日志文件都有唯一的名称，避免了文件覆盖的问题。在`post_processing`方法中，`now`函数再次被用来标记软件处理结束的时间，以及计算软件运行的持续时间。

这种使用方式说明了`now`函数在项目中的重要性，它为日志记录、时间跟踪提供了一种统一且准确的方法。

**注意**: 使用`now`函数时，需要确保系统时间是准确的，因为它依赖于系统的本地时间。此外，返回的时间格式是固定的，如果项目中需要不同的时间格式，可能需要对函数进行相应的修改或者在调用后对返回值进行转换。

**输出示例**: 假设当前时间是2023年4月5日14时30分15秒，`now`函数将返回字符串"20230405143015"。
## FunctionDef log_visualize(role, content)
**log_visualize**: 该函数的功能是将角色和内容发送到可视化服务器，以实时在网页上显示日志。

**参数**:
- role: 发送消息的角色。
- content: 消息的内容。如果未提供，则只发送角色信息。

**代码描述**: `log_visualize` 函数主要用于在开发过程中实时监控日志信息，通过将日志信息发送到可视化服务器，开发者可以在网页上实时查看日志，从而更有效地进行调试和监控。函数接受两个参数：`role` 和 `content`。当只提供了 `role` 参数时，函数会将角色信息作为系统消息发送，并通过 `print` 函数在控制台输出。当同时提供了 `role` 和 `content` 参数时，函数会在控制台打印出完整的日志信息，并调用 `logging.info` 记录日志。如果 `content` 是 `SystemMessage` 类型的实例，函数会将其元数据转换为键值对列表，并使用 `convert_to_markdown_table` 函数将其转换为 Markdown 表格格式，以便更清晰地展示日志信息。最后，无论是否提供了 `content` 参数，函数都会调用 `send_msg` 函数将日志信息发送到可视化服务器。

**注意**:
- 在使用 `log_visualize` 函数时，需要确保可视化服务器已经启动并运行在预期的端口上，以便函数能够成功发送日志信息。
- 当 `content` 参数是 `SystemMessage` 类型时，函数会特别处理，将消息的元数据转换为 Markdown 表格格式，因此在构造 `SystemMessage` 实例时，应确保其元数据字典中的值可以正确转换为字符串。
- 该函数依赖于 `logging` 和 `print` 函数来在控制台输出日志信息，同时依赖于 `send_msg` 函数来发送日志信息到可视化服务器，因此在项目中应确保这些依赖项正确配置和实现。
## FunctionDef convert_to_markdown_table(records_kv)
**convert_to_markdown_table**: 该函数的功能是将键值对列表转换为Markdown格式的表格。

**参数**:
- records_kv: 一个包含键值对的列表，其中每个元素是一个包含两个元素的列表，第一个元素是键（字符串），第二个元素是值（可以是任何可以转换为字符串的类型）。

**代码描述**:
`convert_to_markdown_table`函数接受一个键值对列表作为输入，然后生成一个Markdown格式的表格，其中包含两列，分别是“Parameter”和“Value”。首先，函数创建一个Markdown表格的头部，定义了两列的标题和分隔符。接着，对于输入列表中的每个键值对，函数会创建一个表格的行，其中键作为参数名称，值作为参数值，并且参数名称会被加粗显示。最后，这些行会与表头合并，形成完整的Markdown表格字符串，并返回该字符串。

在项目中，`convert_to_markdown_table`函数被`log_visualize`和`wrapper`两个函数调用。在`log_visualize`函数中，当内容是`SystemMessage`类型时，会将消息的元数据转换为键值对列表，然后使用`convert_to_markdown_table`函数将其转换为Markdown表格，以便以更易读的格式显示在日志中。在`wrapper`函数中，该函数用于将函数调用的参数记录为Markdown表格，以便在系统日志中以结构化的方式展示函数调用的详细信息。这种方式使得日志信息更加清晰、易于理解。

**注意**:
- 输入的键值对列表中的值应该是可以转换为字符串的，因为Markdown表格中的内容都是以文本形式展示的。
- 生成的Markdown表格是以字符串形式返回的，可以直接用于Markdown文件或Markdown支持的平台上展示。

**输出示例**:
假设有以下键值对列表作为输入：
```
[["name", "Alice"], ["age", "30"]]
```
函数将返回以下Markdown表格字符串：
```
| Parameter | Value |
| --- | --- |
| **name** | Alice |
| **age** | 30 |
```
## FunctionDef log_arguments(func)
**log_arguments**: 该函数的功能是装饰另一个函数，以便在调用时自动记录其参数和参数值，并以Markdown表格的形式进行可视化日志记录。

**参数**:
- `func`: 被装饰的函数对象。

**代码描述**:
`log_arguments` 函数是一个装饰器，用于自动记录被装饰函数的参数名称和值。它首先通过`inspect.signature`获取被装饰函数的签名信息，然后结合`*args`和`**kwargs`来收集所有传递给函数的参数和它们的值。这些参数值中，如果参数名为`self`、`chat_env`或`task_type`，则这些参数将被忽略，不会记录，因为它们通常用于内部目的，不需要记录。其余的参数和值将被转义（通过`escape_string`函数）以避免日志中的特殊字符问题，并被记录下来。记录的格式是Markdown表格，通过`convert_to_markdown_table`函数实现。最后，使用`log_visualize`函数将记录以可视化的方式记录下来，其中`"System"`作为日志的发送者。装饰器最终调用原始函数，并返回其返回值。

在项目中，`log_arguments`装饰器被用于`Phase`类的`chatting`方法上。这表明开发者希望自动记录`chatting`方法的调用参数，这对于调试和审计聊天环境设置、用户和助手角色的配置、聊天阶段的提示信息等非常有用。通过这种方式，开发者可以更容易地追踪聊天过程中的各种动态参数，以及它们是如何影响聊天流程的。

**注意**:
- 使用`log_arguments`装饰器时，需要确保被装饰的函数不会修改传入的参数，因为这可能会影响日志记录的准确性。
- 在多线程或异步环境中使用时，需要确保`log_visualize`函数的线程安全或异步兼容，以避免日志记录过程中的竞态条件。

**输出示例**:
由于`log_arguments`装饰器的作用是记录日志，而不直接修改函数的输出，因此没有直接的输出示例。但是，可以预期的日志输出可能是这样的Markdown表格：

```
**[chatting]**

| 参数名                | 参数值         |
|---------------------|--------------|
| task_prompt         | "请构建一个聊天机器人" |
| assistant_role_name | "助手"         |
| user_role_name      | "用户"         |
| phase_prompt        | "聊天阶段提示"    |
| phase_name          | "初步交流"       |
| ...                 | ...          |
```

这个表格将记录`chatting`方法调用时传递的所有参数和它们的值，除了`self`、`chat_env`和`task_type`。
### FunctionDef wrapper
**wrapper**: 该函数的功能是记录函数调用的参数，并以Markdown表格的形式在系统日志中展示。

**参数**:
- *args: 位置参数列表，用于接收传递给被装饰函数的位置参数。
- **kwargs: 关键字参数字典，用于接收传递给被装饰函数的关键字参数。

**代码描述**:
`wrapper` 函数首先通过 `inspect.signature(func)` 获取被装饰函数的签名，然后通过签名获取函数的参数列表。函数将位置参数和关键字参数合并到一个字典中，以便统一处理。对于字典中的每个参数，如果参数名不是 "self"、"chat_env" 或 "task_type"，则将参数值通过 `escape_string` 函数进行转义处理，以确保参数值在Markdown或HTML环境中安全使用。之后，将参数名和处理后的参数值作为键值对列表传递给 `convert_to_markdown_table` 函数，生成Markdown格式的表格字符串。该字符串会被添加到函数名前，形成最终的记录内容。最后，调用 `log_visualize` 函数，将记录内容发送到可视化服务器，并在网页上实时显示日志。

在这个过程中，`escape_string` 函数用于转义字符串，以防止特殊字符影响Markdown格式解析或HTML显示。`convert_to_markdown_table` 函数将键值对列表转换为Markdown表格，使日志信息以结构化的方式展示，更加清晰易读。`log_visualize` 函数负责将处理后的日志信息发送到可视化服务器，实现日志的实时监控。

**注意**:
- 在使用 `wrapper` 函数时，需要确保被装饰的函数参数可以被正确记录和转义。特别是对于非字符串类型的参数，`escape_string` 函数会尝试将其转换为字符串，这可能会引发异常，因此需要确保参数值是可以安全转换为字符串的。
- `wrapper` 函数设计为一个装饰器，用于装饰其他函数，因此在实际使用时，应将其应用于需要记录参数日志的函数上。

**输出示例**:
假设有一个被 `wrapper` 装饰的函数 `example_func(name="Alice", age=30)` 被调用，`wrapper` 函数将生成如下Markdown表格的日志信息并通过 `log_visualize` 函数发送：

```
**[example_func]**

| Parameter | Value |
| --- | --- |
| **name** | Alice |
| **age** | 30 |
```

此日志信息将在可视化服务器上以Markdown表格的形式展示，使开发者能够清晰地看到 `example_func` 函数被调用时传递的参数名和参数值。
***
## FunctionDef escape_string(value)
**escape_string**: 该函数的功能是对输入的字符串进行转义处理，以便安全地在HTML或Markdown环境中使用。

**参数**:
- value: 需要进行转义处理的值。

**代码描述**:
`escape_string`函数首先确保输入值`value`是一个字符串。接着，使用`html.unescape`函数对HTML实体进行解码。之后，通过`markdown.markdown`函数将Markdown格式的文本转换为HTML。紧接着，使用正则表达式`re.sub(r'<[^>]*>', '', value)`移除所有HTML标签，以防止任何HTML代码的执行。最后，将字符串中的换行符`\n`替换为空格，以保持文本的连续性。函数返回处理后的字符串。

在项目中，`escape_string`函数被`log_visualize`和`wrapper`两个函数调用。在`log_visualize`函数中，它用于在将日志内容发送到可视化服务器之前，对内容进行转义处理，确保日志信息在网页上安全显示。在`wrapper`函数中，它用于在记录函数参数日志之前，对参数值进行转义处理，以防止记录的参数值中包含可能影响Markdown格式解析或HTML显示的特殊字符。

**注意**:
- 使用此函数时，需要注意输入值`value`应当是可以转换为字符串的类型，否则在执行`str(value)`时可能会引发异常。
- 转义处理后的字符串将移除所有HTML标签，这意味着如果原始字符串中包含HTML代码，这些代码将不会在最终输出中呈现。

**输出示例**:
假设输入字符串为`"Hello, <b>world</b>! &amp; Welcome."`，则经过`escape_string`函数处理后的返回值将为`"Hello, world! & Welcome."`，其中`<b>`标签被移除，`&amp;`被正确解码为`&`。
