# Deploy/Upgrade Memphis utilizing predefined secrets.

Memphis allows users to utilize predefined Kubernetes secrets containing credentials that remain unchanged during upgrades or other operations. Several variables must be stored in the Kubernetes secret and created before the initial deployment.

#### Step 1: Create a new secret file for Memphis related credentials:

```bash
kubectl create secret generic external-creds -n memphis \
--from-literal=ROOT_PASSWORD=supersecret \
--from-literal=CONNECTION_TOKEN=supersecret \
--from-literal=JWT_SECRET=cHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiT \
--from-literal=REFRESH_JWT_SECRET=cHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiT \
--from-literal=ENCRYPTION_SECRET_KEY=cHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiT \
--from-literal=REFRESH_JWT_SECRET_REST_GW=cHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiT \
--from-literal=JWT_SECRET_REST_GW=cHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiTcHaNgEiT
```

{% hint style="info" %}
Memphis advises creating randomly generated credentials with the following restrictions:

* JWT\_SECRET - comprising a minimum of 128 characters.
* ENCRYPTION\_SECRET\_KEY - comprising of exactly 32 characters.
* ROOT\_PASSWORD - comprising a maximum of 72 characters.
{% endhint %}

#### Step 2: Create an additional secret file for Memphis-metadata. Ensure it is named "memphis-metadata" and kept distinct from the previous one:

```
kubectl create secret generic memphis-metadata -n memphis \
--from-literal=password=cHaNgEiT \
--from-literal=repmgr-password=cHaNgEiT \
--from-literal=admin-password=cHaNgEiT
```

#### Step 3: Deploy Memphis using previously created secrets

```bash
helm install memphis memphis/memphis \
--set memphis.creds.secretConfig.name="external-creds",\
memphis.creds.secretConfig.existingSecret="true",\
global.postgresql.existingSecret="memphis-metadata",\
global.pgpool.existingSecret="memphis-metadata" \
--create-namespace --namespace memphis --wait
```



### Upgrade with pre-defined secret files

#### Step 0: Obtain user-supplied values. <a href="#step-0-obtain-user-supplied-values." id="step-0-obtain-user-supplied-values."></a>

```
helm get values memphis --namespace memphis
```

#### Step 1: Delete the statefulset with cascade=orphan option <a href="#step-2-delete-the-statefulset-with-cascade-orphan-option" id="step-2-delete-the-statefulset-with-cascade-orphan-option"></a>

```bash
kubectl delete statefulset memphis --cascade=orphan -n memphis
```

#### Step 2: Run helm upgrade with all the values you need + updateStrategy=OnDelete <a href="#step-3-run-helm-upgrade-with-all-the-values-you-need-+-updatestrategy-ondelete" id="step-3-run-helm-upgrade-with-all-the-values-you-need-+-updatestrategy-ondelete"></a>

```
helm repo add memphis https://k8s.memphis.dev/charts/ --force-update &&\
helm upgrade --install memphis \
--set memphis.creds.secretConfig.name="external-creds",\
memphis.creds.secretConfig.existingSecret="true",\
global.postgresql.existingSecret="memphis-metadata",\
global.pgpool.existingSecret="memphis-metadata" \
memphis/memphis --create-namespace --namespace memphis --wait
```

#### Step 4: Upgrade brokers. Delete one by one and validate each to return to the online state. <a href="#step-4-upgrade-brokers.-delete-one-by-one-and-validate-each-one-to-get-back-to-the-online-state." id="step-4-upgrade-brokers.-delete-one-by-one-and-validate-each-one-to-get-back-to-the-online-state."></a>
