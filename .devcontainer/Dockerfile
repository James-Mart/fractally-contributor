FROM node:17-alpine

# Install basic dependencies
RUN apk add --no-cache bash  && \
    apk add --no-cache git

# Use bash shell
ENV SHELL /bin/bash

# Need bash shell for ansi quotes
SHELL ["/bin/bash", "-c"]

# Some nice-to-haves when using git inside the container 
# (prettify terminal, git completion)
RUN echo $'\n\
parse_git_branch() {\n\
  git branch 2> /dev/null | sed -e "/^[^*]/d" -e "s/* \\(.*\\)/ (\\1)/"\n\
} \n\
export PS1="\u@\h \W\[\\033[32m\\]\\$(parse_git_branch)\\[\\033[00m\\] $ "\n\
if [ -f ~/.git-completion.bash ]; then\n\
  . ~/.git-completion.bash\n\
fi\n\
 \n\
alias ll="ls -alF"\n\
alias ls="ls --color=auto"\n\
' >> /root/.bashrc

WORKDIR /root

# Args come from the devcontainer.json
ARG GITHUB_ACCOUNT
ARG REPOSITORY_NAME

# Pass the project root name into the container as an environment variable
# If you change this project root, you must also change the workspaceMount in devcontainer.json
ENV PROJROOT="fractally"

# Clone the specified github repo
WORKDIR /root
RUN mkdir ${PROJROOT}
WORKDIR /root/${PROJROOT}
RUN git clone https://github.com/${GITHUB_ACCOUNT}/${REPOSITORY_NAME}.git .

RUN npm i -g pnpm
RUN pnpm i
