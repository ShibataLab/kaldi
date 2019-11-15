
	git clone git@github.com:ShibataLab/kaldi.git
	cd kaldi

# Fork and Maintain changes from original repository
[Source](https://digitaldrummerj.me/git-syncing-fork-with-original-repo/)

	git remote -v
	# origin	git@github.com:ShibataLab/kaldi.git (fetch)
	# origin	git@github.com:ShibataLab/kaldi.git (push)

	git remote add upstream https://github.com/kaldi-asr/kaldi.git
	git remote -v
	# origin	git@github.com:ShibataLab/kaldi.git (fetch)
	# origin	git@github.com:ShibataLab/kaldi.git (push)
	# upstream	https://github.com/kaldi-asr/kaldi.git (fetch)
	# upstream	https://github.com/kaldi-asr/kaldi.git (push)

To get changes from original repository:

	git fetch upstream

Then, make sure you are on 'master' branch:
	
	git checkout master

Merge changes from upstream/master to local master branch

	git merge upstream/master

# Create PyEnv VirtualEnv for Kaldi

	cd kaldi
	pyenv install 3.7.4
	pyenv virtualenv 3.7.4 pytorch-kaldi

# Build and Compile Kaldi

Move to ``./kaldi/tools``

	cd tools

Make sure that there is no error in terms of dependencies

	./extras/check_dependencies.sh

Compile

	make -j 4

Move to ``./kaldi/src``

	cd ../src

Compile

	./configure --shared
	make -j 4 clean depend; make -j 4

# Install PortAudio and Other External Tools

	cd kaldi/tools
	./install_portaudio.sh
	cd ../src
	make -j 4 ext
