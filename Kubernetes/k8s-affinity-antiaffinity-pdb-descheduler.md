# Guida a Kubernetes: Affinity, Anti-Affinity, PDB e Descheduler

## 1. Affinity e Anti-Affinity

### 1.1 Definizione

* **Affinity**: vincola i pod a girare su nodi che soddisfano certe caratteristiche (ad esempio label, topology).
* **Anti-Affinity**: vincola i pod a **non** girare su nodi con certi altri pod o label.

### 1.2 Tipi di Affinity

1. **Node Affinity** – preferenze o requisiti sui nodi.

   * `requiredDuringSchedulingIgnoredDuringExecution` → obbligatorio
   * `preferredDuringSchedulingIgnoredDuringExecution` → preferito

2. **Pod Affinity / Anti-Affinity** – vincoli basati su altri pod.

   * `requiredDuringSchedulingIgnoredDuringExecution` → obbligatorio
   * `preferredDuringSchedulingIgnoredDuringExecution` → preferito

### 1.3 Esempi pratici

**Node Affinity**

```yaml
affinity:
  nodeAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      nodeSelectorTerms:
        - matchExpressions:
            - key: disktype
              operator: In
              values:
                - ssd
```

**Pod Anti-Affinity**

```yaml
affinity:
  podAntiAffinity:
    requiredDuringSchedulingIgnoredDuringExecution:
      - labelSelector:
          matchExpressions:
            - key: app
              operator: In
              values:
                - web
        topologyKey: "kubernetes.io/hostname"
```

---

## 2. Pod Disruption Budget (PDB)

### 2.1 Definizione

Il **PDB** limita il numero di pod che possono essere interrotti contemporaneamente per manutenzioni o aggiornamenti, garantendo disponibilità minima.

### 2.2 Parametri principali

* `minAvailable`: numero minimo di pod disponibili
* `maxUnavailable`: numero massimo di pod indisponibili

### 2.3 Esempio

```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-pdb
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: web
```

---

## 3. Descheduler

### 3.1 Definizione

Il **Descheduler** è un componente opzionale di Kubernetes che sposta i pod per rispettare politiche di affinità, anti-affinità o bilanciamento del cluster, ad esempio dopo aggiornamenti dei nodi.

### 3.2 Strategie comuni

* `RemoveDuplicates`: sposta pod duplicati su nodi diversi
* `LowNodeUtilization`: bilancia risorse tra nodi
* `PodLifeTime`: rimuove pod che vivono da troppo tempo su un nodo
* `RemovePodsViolatingNodeAffinity`: rimuove pod che violano le regole di affinity

### 3.3 Configurazione base

```yaml
apiVersion: "descheduler/v1alpha1"
kind: "DeschedulerPolicy"
strategies:
  "RemovePodsViolatingNodeAffinity":
    enabled: true
    params:
      nodeAffinityType:
        - requiredDuringSchedulingIgnoredDuringExecution
```

---

## 4. Best Practice

1. Combinare **affinity** e **PDB** per garantire disponibilità e resilienza.
2. Usare **anti-affinity** per distribuire carichi critici su nodi diversi.
3. Configurare il **Descheduler** in modo conservativo per evitare continui pod restart.
4. Testare sempre le regole in ambienti di staging prima di applicarle in produzione.

---
