---
layout: post
author: delims
categories: iOS
---

> 以解析一个 big sur 上的 App Store 应用为例，从macOS  11开始自带程序支持 x86_64 和  arm64e

- info.plist 新建 Document Types，class设置为MVDocument
驾驶
- NSDocumentController 调用 openDocumentWithContentsOfURL 触发 MVDocument 调用init方法

- MVDocument 初始化同时创建 dataController ，添加监听三个通知，MVDataTreeChangedNotification、MVDataTableChangedNotification、MVThreadStateChangedNotification

- MVDocument 调用 `- (BOOL)readFromURL:(NSURL *)absoluteURL ofType:(NSString *)typeName error:(NSError **)outError`，把文件copy到临时路径，设置 dataController.realData 为临时文件的全部data，设置 dataController.fileData 为源文件的全部data。
- 接下来 `[dataController createLayouts:dataController.rootNode location:0 length:[dataController.fileData length]]` 创建根节点，读取从location开始的4个字节，判断magic 类型，（fat_magic，mh_magic，mh_magic_64，同时如果是大端需要进行swap），如果是 fat_header ,执行 createFatLayout 并添加到 dataController.layouts 数组，同时根据fat_header 中 nfat_arch 字段遍历接下来的架构 mach_header 加入到跟结点的 children 数组，同时也添加到 dataController.layouts。
- 执行 windowControllerDidLoadNib ，此时dataController.layouts有三个MVLayout ，fat/arm64/x86_64，分别执行这个三个 layouts 的 doMainTasks方法，然后刷新一下 leftView ，接着再开启他们的backgroundThread 
- FatLayout 的 mainTasks 就是生成 header信息，架构数量，架构相关信息，列如：CPU Type，cpu subtype，offset（架构所在文件的索引），size （架构的大小）,align。从 offset 提取 size 大小的字节变成一个新文件，就是一个单架构的完整 mach-o 文件。

- MachOLayout 的 mainTasks 分成好几个步骤。

1.  生成 mach_header_64 信息 ，使用 MATCH_STRUCT(mach_header_64,imageOffset) 宏调用，通过 imageOffset + 起始指针快速定位到mach_header_64结构体数据所在位置，转成结构体赋给结构体mach_header_64变量。取出其中的 ncmds 和 sizeofcmds 然后 createMachONode 创建 mach64_header的Node 
2. 生成 "Load Commands" Node 并加入到根node，当前的根node就是MachOLayout的node ,node 就是左侧显示的可以展开的信息，对用户来说只显示一个node的caption，就是当前结点的名称。
3. 遍历 ncmds 生成 所有的 load\_command的作为"Load Commands" Node 的子node， 每个 load\_command 只有8个字节。cmd 和 cmdsize ，通过  MATCH_STRUCT(load_command,fileOffset)快速取出对应位置的 lc 并添加 commands 数组（vector）中，然后创建当前 lc 结点 ，通过 cmd 获取到 caption ，把 lc 结点 添加到 load\_commmands 的children 数组中。每次循环 fileOffset 往后移动 lc->cmdsize 大小，然后遍历下一个lc。
4. 在遍历到 LC\_SEGMENT\_64 的时候，还会取出其中的 nsects 字段，即当前 LC\_SEGMENT\_64 包含的section数量，然后遍历其中的section，section就排列在 LC\_SEGMENT\_64后面，所有通过 `        uint32_t sectionloc = location + sizeof(struct segment_command_64) + nsect * sizeof(struct section_64);
`就可以获取到第 nesct 个位置的 section，获取sectname作为caption，生成Node添加到LC\_SEGMENT\_64的children数组中。把 section\_64  添加到sections数组中。section\_64 中的fileOffset 指的是在当前 macho内的offset，而不是整个fat文件。
5. 每次生成Node的同时都要生成右侧显示的详细信息保存到node.details

- 随之 windowControllerDidLoadNib 调用。
- 

