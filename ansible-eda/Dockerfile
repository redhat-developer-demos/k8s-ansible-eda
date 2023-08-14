FROM quay.io/rhdevelopers/ansible-eda-base:latest

RUN ansible-galaxy collection install community.general ansible.eda kubernetes.core sabre1041.eda

# Set permissions to support arbitrary user IDs used by OpenShift:
# https://docs.openshift.com/container-platform/4.13/openshift_images/create-images.html#use-uid_create-images
USER root
RUN chown -R 1001:0 /home/ansible && \
    chmod -R ug+rwx /home/ansible

USER ansible

WORKDIR /eda-ansible
COPY --chown=1001:1001 inventory /eda-ansible/inventory
COPY --chown=1001:1001 k8s-eda-rule.yml /eda-ansible/k8s-eda-rule.yml
COPY --chown=1001:1001 k8s-eda-playbook.yml /eda-ansible/k8s-eda-playbook.yml
COPY --chown=1001:1001 k8s-eda-playbook-mail.yml /eda-ansible/k8s-eda-playbook-mail.yml

EXPOSE 5001

CMD ansible-rulebook --rulebook k8s-eda-rule.yml -i inventory -vvv
