# Fork and Maintain changes from original repository
[Source](https://digitaldrummerj.me/git-syncing-fork-with-original-repo/)

Fork original repository in Github.

Clone forked repository.

	git clone git@github.com:ShibataLab/kaldi.git
	cd kaldi

Setup original repository as upstream to get changes from original repository.

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

	pyenv install 3.7.4
	pyenv virtualenv 3.7.4 pytorch-kaldi
	pyenv local pytorch-kaldi

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

# Patch system environment variable PATH to include Kaldi

**Note: Make sure to supply the correct KALDI_ROOT**

	tee ~/.zshrc << END
	export KALDI_ROOT=/home/noel-desktop/Projects/kaldi
	# export IRSTLM=\$KALDI_ROOT/tools/irstlm
	# export LIBLBFGS=\$KALDI_ROOT/tools/liblbfgs-1.10
	# export LD_LIBRARY_PATH=\${LD_LIBRARY_PATH:-}:\${LIBLBFGS}/lib/.libs
	# export SRILM=\$KALDI_ROOT/tools/srilm
	PATH=\$PATH:\$KALDI_ROOT/tools/openfst
	PATH=\$PATH:\$KALDI_ROOT/src/featbin
	PATH=\$PATH:\$KALDI_ROOT/src/gmmbin
	PATH=\$PATH:\$KALDI_ROOT/src/bin
	PATH=\$PATH:\$KALDI_ROOT/src/nnetbin
	PATH=\$PATH:\$KALDI_ROOT/tools/python
	# PATH=\$PATH:\$KALDI_ROOT/tools/kaldi_lm
	# PATH=\$PATH:\$IRSTLM/bin
	# PATH=\$PATH:\$SRILM/bin:\$SRILM/bin/i686-m64
	export PATH
	END

# Close terminal and test if PATH has been properly patched.

``copy-feats`` should show something like this...

	copy-feats 
	
	Copy features [and possibly change format]
	Usage: copy-feats [options] <feature-rspecifier> <feature-wspecifier>

``hmm-info`` should show something like this...

	hmm-info 

	Write to standard output various properties of HMM-based transition model