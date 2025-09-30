ArgoCD [seems to be having trouble correctly using gpg subkeys](https://github.com/argoproj/argo-cd/issues/4930). Despite what [this guy](https://github.com/argoproj/argo-cd/issues/4930#issuecomment-3094613823), that was not sufficient to get the signature verification to work. The main problem in my case was, that the argocd pods seem to only load the gpg keys during startup. So each time the gpg keys are modified, the pods need to be restarted (hello, reloader!).

Exporting the subkey public key is equivalent to exporting the primary public key, you get a bundle of all the pub keys.
```
gpg -k --with-subkey-fingerprints
gpg --export --armor <SUBKEY_FINGERPRINT>
```

- Funnily enough, the exported key is the same on the gpg side (unless we use a `!` at the end of the fingerprint, which explicitly only exports that key, see [here](https://www.gnupg.org/(en)/documentation/manuals/gnupg24/gpg.1.html) under "HOW TO SPECIFY A USER ID")
- But ArgoCD seems to insist, that the subkey actually has a fingerprint belonging to its Primary Key, and will refuse to swallow it (run `argocd gpg list` to verify). For now we'll make do with using the primary key...
- Validation of gpg signatures does seem to work properly though...

The gpg key needs to be stored in the `argocd-gpg-keys-cm` under a key  named after its fingerprint, [see](https://github.com/argoproj/argo-helm/blob/a640fb3cba594b62ec38b72cb32883506faffeca/charts/argo-cd/values.yaml#L527)
Validate gpg key is available in argocd.

```
argocd gpg list
```


```
git clone https://github.com/MausoleumManagement/argocd.git
cd argocd

GNUPGHOME=/app/config/gpg/keys git log --show-signature
GNUPGHOME=/app/config/gpg/keys gpg --list-keys
```
