
## gulp.src(globs[, options])

输出（Emits）符合所提供的匹配模式（glob）或者匹配模式的数组（array of globs）的文件。 将返回一个 Vinyl files 的 stream 它可以被 piped 到别的插件中。

**globs**

类型： String 或 Array

所要读取的 glob 或者包含 globs 的数组。

Gulp内部使用了node-glob模块来实现其文件匹配功能。我们可以使用下面这些特殊的字符来匹配我们想要的文件：

- * 匹配文件路径中的0个或多个字符，但不会匹配路径分隔符，除非路径分隔符出现在末尾
- ** 匹配路径中的0个或多个目录及其子目录,需要单独出现，即它左右不能有其他东西了。如果出现在末尾，也能匹配文件。
- ? 匹配文件路径中的一个字符(不会匹配路径分隔符)
- [...] 匹配方括号中出现的字符中的任意一个，当方括号中第一个字符为^或!时，则表示不匹配方括号中出现的其他字符中的任意一个，类似js正则表达式中的用法
- !(pattern|pattern|pattern) 匹配任何与括号中给定的任一模式都不匹配的
- ?(pattern|pattern|pattern) 匹配括号中给定的任一模式0次或1次，类似于js正则中的(pattern|pattern|pattern)?
- +(pattern|pattern|pattern) 匹配括号中给定的任一模式至少1次，类似于js正则中的(pattern|pattern|pattern)+
- *(pattern|pattern|pattern) 匹配括号中给定的任一模式0次或多次，类似于js正则中的(pattern|pattern|pattern)*
- @(pattern|pattern|pattern) 匹配括号中给定的任一模式1次，类似于js正则中的(pattern|pattern|pattern)

下面以一系列例子来加深理解

- * 能匹配 a.js,x.y,abc,abc/,但不能匹配a/b.js
- *.* 能匹配 a.js,style.css,a.b,x.y
- */*/*.js 能匹配 a/b/c.js,x/y/z.js,不能匹配a/b.js,a/b/c/d.js
- ** 能匹配 abc,a/b.js,a/b/c.js,x/y/z,x/y/z/a.b,能用来匹配所有的目录和文件
- **/*.js 能匹配 foo.js,a/foo.js,a/b/foo.js,a/b/c/foo.js
- a/**/z 能匹配 a/z,a/b/z,a/b/c/z,a/d/g/h/j/k/z
- a/**b/z 能匹配 a/b/z,a/sb/z,但不能匹配a/x/sb/z,因为只有单**单独出现才能匹配多级目录
- ?.js 能匹配 a.js,b.js,c.js
 - a?? 能匹配 a.b,abc,但不能匹配ab/,因为它不会匹配路径分隔符
- [xyz].js 只能匹配 x.js,y.js,z.js,不会匹配xy.js,xyz.js等,整个中括号只代表一个字符
- [^xyz].js 能匹配 a.js,b.js,c.js等,不能匹配x.js,y.js,z.js

**options**

类型： Object

通过 glob-stream 所传递给 node-glob 的参数。

除了 node-glob 和 glob-stream 所支持的参数外，gulp 增加了一些额外的选项参数：

options.buffer(类型： Boolean 默认值： true)：如果该项被设置为 false，那么将会以 stream 方式返回 file.contents 而不是文件 buffer 的形式。这在处理一些大文件的时候将会很有用。**注意：**插件可能并不会实现对 stream 的支持。

options.read(类型： Boolean 默认值： true)：如果该项被设置为 false， 那么 file.contents 会返回空值（null），也就是并不会去读取文件。

options.base(类型： String 默认值： 将会加在 glob 之前)

	gulp.src('client/js/**/*.js') // 匹配 'client/js/somedir/somefile.js' 并且将 `base` 解析为 `client/js/`
	  .pipe(minify())
	  .pipe(gulp.dest('build'));  // 写入 'build/somedir/somefile.js'
	
	gulp.src('client/js/**/*.js', { base: 'client' })
	  .pipe(minify())
	  .pipe(gulp.dest('build'));  // 写入 'build/js/somedir/somefile.js'
