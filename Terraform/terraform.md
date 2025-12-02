
# 🟦 Terraform

### 1. Werk je Ubuntu server bij

```bash
sudo apt update
sudo apt upgrade -y
```

**Best practice:** houd je omgeving up-to-date om compatibiliteitsproblemen te voorkomen.

---

## 🟦 2. Terraform installeren (officiële methode)

Gebruik altijd de officiële installatie-instructies:

🔗 [https://developer.hashicorp.com/terraform/install](https://developer.hashicorp.com/terraform/install)

### 2.1 HashiCorp GPG sleutel toevoegen

```bash
sudo apt update
sudo apt install -y gnupg software-properties-common curl

curl -fsSL https://apt.releases.hashicorp.com/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/hashicorp.gpg
```

### 2.2 HashiCorp repository toevoegen

```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" \
| sudo tee /etc/apt/sources.list.d/hashicorp.list

sudo apt update
```

### 2.3 Terraform installeren

```bash
sudo apt install terraform -y
```

### 2.4 Controleren

```bash
terraform -version
```

---

## 🟦 3. Projectmap voorbereiden

### Map aanmaken

```bash
mkdir ~/terraform-project
cd ~/terraform-project
```

### Hoofdconfiguratie maken

```bash
nano main.tf
```

### Bestanden controleren

```bash
ls -l
```

---

## 🟦 4. Gebruik van variables.tf en terraform.tfvars

Terraform gebruikt variabelen om configuraties flexibel en overzichtelijk te houden.

### 4.1 Maak een `variables.tf`

```hcl
variable "region" {}
variable "project_name" {}
```

### 4.2 Maak `terraform.tfvars` voor echte waarden

```hcl
region = "eu-west-1"
project_name = "student-app"
```

**Best practices:**

* Zet **nooit gevoelige data** in `main.tf`.
* Voeg `terraform.tfvars` toe aan `.gitignore`.
* Gebruik eventueel `terraform.tfvars.example` zonder echte geheimen.

---

## 🟦 5. Terraform initialiseren

```bash
terraform init
```

---

## 🟦 6. Plan uitvoeren

```bash
terraform plan
```

Geeft een overzicht van welke resources Terraform zal aanmaken, wijzigen of verwijderen.

---

## 🟦 7. Deploy uitvoeren

```bash
terraform apply
```

---

## 🟦 8. Resources bekijken

```bash
terraform state list
```

---

## 🟦 9. Destroy uitvoeren

```bash
terraform destroy
```

---

## 🟦 🔐 10. Best practices (Security & Stability)

### 🔐 Security

* Gebruik `terraform.tfvars` of environment variables voor gevoelige gegevens.
* Commit nooit `terraform.tfstate` → bevat vaak gevoelige info.
* Overweeg remote backends zoals:

  * Terraform Cloud
  * AWS S3 + DynamoDB
  * Azure Blob Storage

---

### 🛠️ Stabiliteit

Gebruik provider-versiepinnen:

```hcl
required_providers {
  aws = {
    source  = "hashicorp/aws"
    version = "~> 5.0"
  }
}
```

---

### 📁 Onderhoud

Voer regelmatig uit:

```bash
terraform plan
```

Documenteer modules en houd een overzichtelijke structuur aan:

```
/modules
/environments/dev
/environments/prod
```

---
