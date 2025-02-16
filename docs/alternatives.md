# Distribution Unix

## Suite office
- [OnlyOffice](https://github.com/ONLYOFFICE/DesktopEditors)
- [Collabora](https://github.com/CollaboraOnline/online)

## Messagerie Instantanée
- [Wire](https://github.com/wireapp/wire)
- [Element](https://github.com/element-hq) - basé sur le protocole Matrix
- [Twake](https://github.com/linagora/twake-on-matrix) - fait partie du projet Twake Workplace

## Client mail avec spécificités
De préférence, cliant intégrant le protocole JMAP
- [Tmail](https://github.com/linagora/tmail-flutter) - fait partie du projet Twake Workplace

## Calendrier qui s'intègre avec la messagerie instantanée et le client mail
## Contacts qui s'intègre avec la messagerie instantanée et le client mail

## Drive : client sur le poste qui synchronise les fichiers et une interface web
- [ceph client](https://github.com/ceph/ceph) à configurer
Interface web à développer

## Authentification Linux sous PAM, avec authentification hors ligne et forte
- [Client PAM KaniDM](https://github.com/kanidm/kanidm)

## Agent d'un serveur d'automatisation d'une flotte
Script bash qui pull configuration.nix à intervalle régulier
- [Puppet](https://github.com/puppetlabs/puppet)
- [Chef](https://github.com/chef/chef)
- [Salt](https://github.com/saltstack/salt)
- [fleet](https://github.com/fleetdm/fleet)

## Logiciel de sécurité intégré (XDR)
- [ossec-hids](https://github.com/ossec/ossec-hids)
- [wazuh](https://github.com/wazuh/wazuh)

## Client VPN
- [Client OpenVPN Linux](https://github.com/OpenVPN/openvpn3-linux)

## Outil d'encryption
- [Picocrypt](https://github.com/Picocrypt/Picocrypt)

## Navigateur avec extension bloqueur de pub
- [Mozilla](https://hg.mozilla.org/) avec
[uBlock Origin](https://github.com/gorhill/uBlock) et
[Bitwarden](https://github.com/bitwarden/clients)

## Firewall local
iptables ou nftables ou ufw

## Editeur de texte
- [vscodium](https://github.com/VSCodium/vscodium)
- [zed](https://github.com/zed-industries/zed)

## Bureau à distance
[Rustdesk](https://github.com/rustdesk/rustdesk) - soumis à une licence pour la version Pro

# Services cloud à héberger

## Système de fichiers
- [rook](https://github.com/rook/rook) managing [ceph](https://github.com/ceph/ceph)

## Service de mail
SMTP Server with [JMAP](https://jmap.io/) support like :
- [StalwartLabs Mail Server](https://github.com/stalwartlabs/mail-server) or
- [Apache James](https://github.com/apache/james-project)

## Services de monitoring (Grafana)
- [Grafana](https://github.com/grafana/grafana)

## Services de logging (Prometheus & Loki)
- [Prometheus](https://github.com/prometheus/prometheus)
 et [Loki](https://github.com/grafana/loki) for logs

## Services de backup (velero)
[velero](https://github.com/vmware-tanzu/velero) - cluster
et [ceph](https://github.com/ceph/ceph) - fs


## Services de sécurité
[Kyverno](https://github.com/kyverno/kyverno) - Policy Enforcement

[Falco](https://github.com/falcosecurity/falco) - Runtime Security

[Trivy](https://github.com/aquasecurity/trivy) - Vulnerability Scanning

[Kubescape](https://github.com/kubescape/kubescape) - Compliance and Hardening

[Cilium](https://github.com/cilium/cilium) - Network Security

## Services de gestion des identités
- [KaniDM](https://github.com/kanidm/kanidm) - Authentification et permissions à plusieurs niveaux : LDAP, RADIUS, OAUTH

## Services de registre des images contenarisées (Harbor)
- [harbor](https://github.com/goharbor/harbor)

## Service d'automatisation d'une flotte
Symétrie avec l'agent d'automatisation

## Service de registers des paquets Node (et Java?)

## Services de ticketting

## Service VPN
- [OpenVPN](https://github.com/OpenVPN/openvpn) avec [plugin OAuth](https://github.com/jkroepke/openvpn-auth-oauth2)

## Service de gestion des mots de passe
- [vaultwarden](https://github.com/dani-garcia/vaultwarden)
- [bitwarden](https://github.com/bitwarden/server)

## Espace drive perso d'une taille fixée

## Logiciel de signature en ligne
- [Docuseal](https://github.com/docusealco/docuseal)

## Logiciel de wiki
- [wiki](https://github.com/requarks/wiki)

## Logiciel de discussion communautaire
- [Discourse](https://github.com/discourse/discourse)

## Service de DNS
Pour avoir des enregistrements DNS locaux pour les services
- [CoreDNS](https://github.com/coredns/coredns)


## Service de proxy
- [Traefik](https://github.com/traefik/traefik)
- [HAProxy](https://github.com/haproxy/haproxy)
- [Envoy](https://github.com/envoyproxy/envoy)
