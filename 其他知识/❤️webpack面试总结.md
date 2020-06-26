# webpack面试总结

## webpack plugins & loaders

- 图片压缩 — `image-webpack-loader`

- 多个icons合成雪碧图 — `webpack-spritesmith`

- 压缩js — `uglifyjs-webpack-plugin`

- 压缩css — `optimize-css-assets-webpack-plugin`

- 抽离css — `extract-text-webpack-plugin`（在`webpack4`中，建议用`mini-css-extract-plugin`替代）

- `file-loader`

  生成文件，输出到输出目录并返回 public URL。

- `url-loader` 

  功能类似于`file-loader`，但是在文件大小（单位 byte）低于指定的限制时，可以返回一个 DataURL。

- `html-webpack-plugin`

  创建HTML页面文件到你的输出目录并且将webpack打包后的chunk自动引入到这个HTML中

- `webpack-dev-server`

  静态资源服务器，hot设为true可使用热重载功能，但需额外引两个插件webpack.HotModuleReplacementPlugin 及 webpack.NoEmitOnErrorsPlugin

  

## plugin & loader区别

- *loader* 让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。
- loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。[插件接口](https://www.webpackjs.com/api/plugins)功能极其强大，可以用来处理各种各样的任务。