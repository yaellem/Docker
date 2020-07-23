FROM debian:9.8
ENV TERM xterm
RUN apt-get update && apt-get install -y \
		curl \
		openssh-server \
		ca-certificates \
		postfix

RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN echo 'root:coucou' | chpasswd

RUN curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.deb.sh | bash
RUN apt-get install -y gitlab-ce \
	&& rm -rf /var/lib/apt/lists/*

ARG IP
RUN mkdir -p /etc/gitlab/ssl \
	&& chmod 700 /etc/gitlab/ssl \
	&& openssl req -x509 -nodes -days 365 -newkey rsa:4096 \
		-subj /C=FR/ST=75/L=Paris/O=rs1/OU=rs1/CN=$IP/emailAddress=ymarcill@student.42.fr \
		-keyout /etc/gitlab/ssl/selfsigned.key -out /etc/gitlab/ssl/selfsigned.crt
RUN echo "THE IP IS : " $IP 
RUN echo "external_url \"https://$IP\"" > /etc/gitlab/gitlab.rb
RUN echo "nginx['redirect_http_to_https'] = true" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate'] = \"/etc/gitlab/ssl/selfsigned.crt\"" >> /etc/gitlab/gitlab.rb
RUN echo "nginx['ssl_certificate_key'] = \"/etc/gitlab/ssl/selfsigned.key\"" >> /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['gitlab_shell_ssh_port'] = 2222" >> /etc/gitlab/gitlab.rb
#    sed -i "s/# prometheus_monitoring\['enable'\] = true/prometheus_monitoring\['enable'\] = false/g" /etc/gitlab/gitlab.rb && \
#RUN echo "grafana['enable'] = false" >> /etc/gitlab/gitlab.rb
#RUN echo "puma['enable'] = false" >> /etc/gitlab/gitlab.rb
#RUN echo "unicorn['enable'] = true" >> /etc/gitlab/gitlab.rb
#RUN echo "unicorn['worker_timeout'] = 3000" >> /etc/gitlab/gitlab.rb
#RUN echo "prometheus_monitoring['enable'] = false" >> /etc/gitlab/gitlab.rb
#EXPOSE 443 80 22 2222
ENTRYPOINT service ssh start \
	&& (/opt/gitlab/embedded/bin/runsvdir-start &) \
	&& gitlab-ctl reconfigure \
	&& /bin/bash

