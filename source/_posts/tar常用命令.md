---
title: tar常用命令
date: 2021-05-28 15:08:32
tags: [服务器]
---

# 参数含义

- -c：创建新的存档文件（Create）
- -x：从存档文件中提取文件（eXtract）
- -t：列出存档文件中的内容（lisT）
- -v：显示 tar 命令执行的详细信息（Verbose）
- -f：指定存档文件的名称（File）
- -z：在创建或提取存档文件时使用 gzip 压缩算法来进行压缩或解压缩（gzip）
- -j：在创建或提取存档文件时使用 bzip2 压缩算法来进行压缩或解压缩（bzip2）
- -C：指定 tar 命令的工作目录（Change directory）

# 常用命令

|算法|参数|压缩|解压|特点|
|-|-|-|-|-|
|无|N/A|tar -cvf archive.tar directory/|tar -xvf archive.tar|N/A|
|gzip|-z|tar -czvf archive.tar.gz directory/|tar -xzvf archive.tar.gz|gzip 是 GNU 压缩程序，它是基于 DEFLATE 算法的一种压缩方式，压缩速度快，压缩率较高|
|bzip2|-j|tar -cjvf archive.tar.bz2 directory/|tar -xjvf archive.tar.bz2|bzip2 使用 Burrows-Wheeler 算法，压缩率比 gzip 高，但压缩速度较慢|
|LZMA/LZMA2|-J|tar -cJvf archive.tar.xz directory/|tar -xJvf archive.tar.xz|xz 使用 LZMA/LZMA2 算法，提供更高的压缩率，但压缩和解压缩速度相对较慢|
|compress|-Z|tar -cZvf archive.tar.Z directory/|tar -xZvf archive.tar.Z|compress 是早期的 UNIX 压缩工具，基于 LZW 算法，压缩率和速度都较低，现已很少使用|
|lzip|--lzip|tar --lzip -cvf archive.tar.lz directory/|tar --lzip -xvf archive.tar.lz|lzip 使用 LZMA 算法，提供高压缩率，压缩和解压速度较慢|
|lzop|--lzop|tar --lzop -cvf archive.tar.lzo directory/|tar --lzop -xvf archive.tar.lzo|lzop 基于 LZO 算法，压缩速度非常快，但压缩率较低|
|zstd|--zstd|tar --zstd -cvf archive.tar.zst directory/|tar --zstd -xvf archive.tar.zst|zstd 是由 Facebook 开发的一种压缩算法，提供高压缩率和快速的压缩/解压速度|
