ArgoCD [seems to be having trouble correctly using gpg subkeys](https://github.com/argoproj/argo-cd/issues/4930). Despite what [this guy](https://github.com/argoproj/argo-cd/issues/4930#issuecomment-3094613823) claimed, i couldn't get the subkey to work, no matter which fingerprint (master, subkey, long, short) i used for exporting and configuring it in the ConfigMap.


What we would like
```
gpg -k --with-subkey-fingerprints
gpg --export --armor <SUBKEY_FINGERPRINT>
```
What we are doing
```
gpg -k
gpg --export --armor <PRIMARY_KEY_FINGERPRINT>
```

Funnily enough, the exported key is the same on the gpg side (unless we use a `!` at the end of the fingerprint, which explicitly only exports that key, see [here](https://www.gnupg.org/(en)/documentation/manuals/gnupg24/gpg.1.html) under "HOW TO SPECIFY A USER ID"). But ArgoCD gets seems to insist, that the subkey actually has a fingerprint belonging to its Primary Key, and will refuse to swallow it (run `argocd gpg list` to verify). For now we'll make do with using the primary key...

The gpg key needs to be stored in the `argocd-gpg-keys-cm` under a key  named after its fingerprint, [see](https://github.com/argoproj/argo-helm/blob/a640fb3cba594b62ec38b72cb32883506faffeca/charts/argo-cd/values.yaml#L527)
Validate gpg key is available in argocd.

```
argocd gpg list
```
