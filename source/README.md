首先删除hexo 下面的.deploy_git文件夹，然后运行

git config --global core.autocrlf false

重新 hexo clean,hexo g,hexo d就行了