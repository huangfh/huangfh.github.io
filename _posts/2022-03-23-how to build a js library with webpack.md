---
layout: post
read_time: true
show_date: true
title: "webpack library 打包 js 库"
date: 2022-02-24
img: webpack.jpeg
tags: [js library,webpack]
author: huangfh
description: ""
toc: yes
---
webpack library 


```javascript
const path = require('path');
const webpack = require('webpack');
const PrettierPlugin = require("prettier-webpack-plugin");
const TerserPlugin = require('terser-webpack-plugin');
const getPackageJson = require('./scripts/getPackageJson');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");

const {
  version,
  name,
  license,
  repository,
  author,
} = getPackageJson('version', 'name', 'license', 'repository', 'author');

const banner = `
  ${name} v${version}
  ${repository.url}

  Copyright (c) ${author.replace(/ *\<[^)]*\> */g, " ")} and project contributors.

  This source code is licensed under the ${license} license found in the
  LICENSE file in the root directory of this source tree.
`;

module.exports = {
  mode: "production",
  devtool: 'source-map',
  entry: './src/lib/index.js',
  output: {
    filename: 'index.js',
    path: path.resolve(__dirname, 'build'),
    // library名称
    library: 'highlight',
    // 模块规范
    libraryTarget: 'umd',
    // 模块导出的方式。此处设置为 export default方式
    libraryExport: "default",
    clean: true
  },
  optimization: {
    minimize: true,
    minimizer: [
      new TerserPlugin({ extractComments: false }),
      new OptimizeCSSAssetsPlugin({
        cssProcessorOptions: {
          map: {
            inline: false
          }
        }
      }),
    ]
  },
  module: {
    rules: [
      {
        test: /\.m?js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader'
        }
      },
      {
        test: /\.(sa|sc|c)ss$/,
        use: [
          MiniCssExtractPlugin.loader,
          { loader: "css-loader", options: { sourceMap: true } },
        ],
      }
    ]
  },
  plugins: [
    new PrettierPlugin(),
    new MiniCssExtractPlugin({
      filename: 'index.css'
    }),
    new webpack.BannerPlugin(banner)
  ]
};
```
