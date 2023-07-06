FROM registry.access.redhat.com/ubi9-minimal
RUN microdnf install java-17 python3 gcc python3-devel -y && microdnf clean all && python -m ensurepip --upgrade && pip3 install ansible ansible-rulebook asyncio aiokafka aiohttp aiosignal kubernetes requests ansible-runner wheel
ENV JAVA_HOME="/usr/lib/jvm/jre-17"
RUN mkdir /eda-ansible

RUN ansible-galaxy collection install community.general ansible.eda kubernetes.core sabre1041.eda

COPY inventory /eda-ansible/inventory
COPY k8s-eda-rule.yml /eda-ansible/k8s-eda-rule.yml
COPY k8s-eda-playbook.yml /eda-ansible/k8s-eda-playbook.yml
COPY k8s-eda-playbook-mail.yml /eda-ansible/k8s-eda-playbook-mail.yml

WORKDIR /eda-ansible
EXPOSE 5001

CMD ["ansible-rulebook", "--rulebook", "k8s-eda-rule.yml", "-i", "inventory", "--verbose"]