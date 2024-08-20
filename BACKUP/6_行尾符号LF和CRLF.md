# [行尾符号LF和CRLF](https://github.com/Daotin/issue-blog/issues/6)

## 一般在不同的系统，行业通用的做法是如何使用行尾符号的？

在不同操作系统上，行尾符号的使用有一定的行业惯例和最佳实践。以下是不同系统以及在跨平台开发中常见的行尾符号使用方式：

### 不同操作系统的行尾符号惯例

1. **Unix/Linux 和 macOS**：
   - 使用 LF (Line Feed)，即 `\n` 作为行尾符号。
   - 这是大多数服务器和开发环境的默认行尾符号。

2. **Windows**：
   - 使用 CRLF (Carriage Return and Line Feed)，即 `\r\n` 作为行尾符号。
   - Windows上的大多数文本编辑器默认使用这种行尾符号。

### 行业通用的做法

1. **统一行尾符号**：
   - 在跨平台项目中，统一使用LF行尾符号是比较常见的做法，因为大多数现代版本控制系统和代码编辑器都可以很好地处理LF行尾符号。
   - 可以在项目根目录下创建 `.editorconfig` 文件，配置行尾符号格式：
     ```ini
     root = true

     [*]
     end_of_line = lf
     charset = utf-8
     ```

2. **Git的行尾符号转换**：
   - Git 提供了 `core.autocrlf` 选项来处理行尾符号转换：
     - `core.autocrlf=true`：在Windows上检出代码时将LF转换为CRLF，提交代码时将CRLF转换为LF。
     - `core.autocrlf=input`：在检出代码时不转换行尾符号，提交代码时将CRLF转换为LF。这种设置适用于开发人员在Unix/Linux或macOS系统上。
     - `core.autocrlf=false`：不进行任何转换。这种设置适用于所有开发人员都使用统一的行尾符号，例如通过`.editorconfig`来强制执行LF行尾符号。

   例如，在Windows系统上可以设置：
   ```sh
   git config --global core.autocrlf true
   ```
   在Unix/Linux或macOS系统上可以设置：
   ```sh
   git config --global core.autocrlf input
   ```

3. **.gitattributes 文件**：
   - 通过 `.gitattributes` 文件来强制特定文件类型使用特定的行尾符号。
   - 在项目根目录下创建 `.gitattributes` 文件，内容如下：
     ```text
     * text=auto
     *.sh text eol=lf
     *.bat text eol=crlf
     ```
   - 这可以确保Shell脚本使用LF行尾符号，而批处理文件使用CRLF行尾符号。

### 总结

行业通用的做法是尽量统一使用LF行尾符号，尤其是在跨平台项目中，通过配置`.editorconfig`、`.gitattributes`文件和Git的`core.autocrlf`选项，可以有效地管理和处理行尾符号，确保代码在不同操作系统上的一致性和兼容性。

## 如何配置统一使用LF行尾符号？

要统一使用LF行尾符号，可以通过配置`.editorconfig`文件、`.gitattributes`文件以及Git的`core.autocrlf`选项来实现。以下是具体步骤：

### 1. 使用 `.editorconfig` 文件

`.editorconfig` 文件可以帮助你在不同编辑器和IDE中统一代码风格，包括行尾符号。

在项目根目录下创建一个名为 `.editorconfig` 的文件，并添加以下内容：

```ini
root = true

[*]
end_of_line = lf
charset = utf-8
```

这将告诉支持 EditorConfig 的编辑器使用 LF 作为所有文件的行尾符号，并使用 UTF-8 编码。

### 2. 配置 `.gitattributes` 文件

在项目根目录下创建一个名为 `.gitattributes` 的文件，并添加以下内容：

```text
* text=auto
*.sh text eol=lf
*.js text eol=lf
*.css text eol=lf
*.html text eol=lf
```

这将确保在 Git 中所有文件使用自动检测的文本模式，而对于特定类型的文件（例如 `.sh`、`.js`、`.css`、`.html`），强制使用 LF 作为行尾符号。

### 3. 配置 Git 的 `core.autocrlf` 选项

根据操作系统的不同，配置 Git 的 `core.autocrlf` 选项：

- **在 Unix/Linux 和 macOS 系统上**：

  ```sh
  git config --global core.autocrlf input
  ```

  这将告诉 Git 检出代码时不转换行尾符号，提交代码时将 CRLF 转换为 LF。

- **在 Windows 系统上**：

  ```sh
  git config --global core.autocrlf true
  ```

  这将告诉 Git 检出代码时将 LF 转换为 CRLF，提交代码时将 CRLF 转换为 LF。

### 4. 将现有文件转换为 LF 行尾符号

如果你有现有的文件需要转换为 LF 行尾符号，可以使用以下命令进行批量转换：

- **在 Unix/Linux 和 macOS 系统上**：

  ```sh
  find . -type f -exec dos2unix {} \;
  ```

- **在 Windows 系统上**：

  使用 Git Bash 或其他 Unix 风格的终端运行以下命令：

  ```sh
  find . -type f -exec dos2unix {} \;
  ```

或者使用 PowerShell 脚本：

```powershell
Get-ChildItem -Recurse -File | ForEach-Object {
    (Get-Content -Raw -Path $_.FullName).Replace("`r`n", "`n") | Set-Content -NoNewline -Path $_.FullName
}
```

### 总结

通过上述步骤，你可以在项目中统一使用 LF 行尾符号。使用 `.editorconfig` 文件和 `.gitattributes` 文件可以确保编辑器和 Git 都按照统一的行尾符号处理文件，从而避免由于行尾符号不同导致的问题。


## 如果当前是CRLF，并且设置了 `git config --global core.autocrlf true` ，会发生什么？

如果当前的文件使用的是CRLF行尾符号，并且你在Git中设置了`git config --global core.autocrlf true`，会发生以下情况：

1. **检出文件时**：
   - Git会将仓库中的LF行尾符号转换为CRLF行尾符号。这是因为`core.autocrlf`配置为`true`会自动在Windows系统上将所有检出的文件转换为CRLF，以适应Windows系统的行尾符号惯例。

2. **提交文件时**：
   - Git会将工作目录中的CRLF行尾符号转换为LF行尾符号，然后将文件提交到仓库中。这意味着，尽管你的工作目录中的文件使用的是CRLF行尾符号，但一旦提交到Git仓库中，它们将被转换为LF行尾符号存储。

### 具体影响

1. **避免行尾符号问题**：
   - 这种配置可以避免在跨平台开发中常见的行尾符号问题。例如，当Windows用户和Unix/Linux或macOS用户共同开发一个项目时，可以确保仓库中的行尾符号一致，避免因行尾符号不同而导致的合并冲突和差异问题。

2. **文件内容一致性**：
   - 尽管在Windows上工作目录中的文件使用的是CRLF，但由于在提交时Git会将其转换为LF，因此仓库中存储的文件行尾符号始终是LF。这保证了文件内容的一致性。

3. **编辑器的影响**：
   - Windows上的一些文本编辑器（如Notepad）对LF行尾符号支持不佳，可能导致显示和编辑问题。而配置`core.autocrlf=true`可以让检出的文件使用CRLF，从而避免这些问题。

### 需要注意的事项

1. **现有文件的转换**：
   - 如果项目中已有大量文件使用的是CRLF行尾符号，在配置`core.autocrlf=true`后，第一次提交时可能会看到大量文件被修改，因为Git会将它们的行尾符号转换为LF。

2. **提交前检查**：
   - 在第一次提交前，可以使用`git status`和`git diff`检查哪些文件将被修改，以确保没有意外的变更。

3. **团队协作**：
   - 确保团队中的所有成员都了解并统一使用相同的Git配置，以避免由于行尾符号处理不一致导致的问题。

### 总结

配置`git config --global core.autocrlf true`可以帮助在Windows系统上工作的开发者自动处理行尾符号转换问题，确保提交到Git仓库中的文件使用一致的LF行尾符号。这种配置在跨平台开发中非常有用，但需要团队成员了解这一配置，并可能需要对现有文件进行一次性处理以统一行尾符号。