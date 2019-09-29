# czh
个人博客
npm install hexo-cli -g
hexo init blog
cd blog
npm install
hexo server

_config.yml:
	deploy:
	type: git
	repository: https://github.com/liboyue23/my-hexo.git
	branch: gh-pages

npm install hexo-deployer-git --save

hexo g
hexo d