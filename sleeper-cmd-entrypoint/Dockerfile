#Pull base image.
FROM ubuntu:14.04

# Install.
RUN \
  sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list && \
  apt-get update && \
  apt-get -y upgrade && \
  apt-get install -y build-essential && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget && \
  rm -rf /var/lib/apt/lists/*

# Add files.
ADD .bashrc /root/.bashrc
ADD .gitconfig /root/.gitconfig


# Set environment variables.
ENV HOME /root

# Define working directory.
WORKDIR /root

# Define default command.
# CMD ["bash"]
# # this would run the CMD as soon as the container runs
# CMD sleep 10 
# This would be same as the CMD instruction and whatever we specify in the commandline will get appended to the ENTRYPOINT.
ENTRYPOINT [ "sleep" ]
## We can add a default time as CMD after entrypoint will append right next to the entrypoint command
CMD [ "5" ]