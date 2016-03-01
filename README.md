根据erlang自定义behaviour生成编译依赖关系

erlang中可以自定义behaviour,但是如果一个模块是实现了该自定义behaviour的话,那么首先要编译behaviour,其次在编译该模块时加入-pa指定编译好的behaviour.beam路径,否则编译的时候会提示该behaviour未定义.

换句话说,实现需要依赖到behaviour模块编译.

因此需要根据代码中的这种依赖关系,对代码进行一次梳理,定义一个正确的编译顺序.幸而rabbitmq项目已经有这样的代码,我就抽取出来用了,具体的实现还没有细看.在这个框架的文件组织中,erl文件放在src子目录下面,在src/Makefile中,首先以当前目录的所有erl文件为参数调用../generate_deps(这个文件取自rabbitmq项目)来产生依赖关系文件deps.mk,注意要在Makefile中include这个文件,这样新的依赖关系才会起效,然后编译会自动根据新的依赖关系来安排编译的顺序了.有了这些,你所需要做的就是把你写的erl文件放在src目录下面,依赖关系什么的,由Makefie搞定了.

如果是用rebar来构建项目的话，可以再rebar.config中加入下面这行，指定需要编译的erl文件
{erl_first_files, ["emangosd_protocol"]}

http://www.codedump.info/?p=261
