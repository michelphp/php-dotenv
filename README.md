> ⚠️ **Abandoned package**
>
> This package is abandoned and no longer maintained.  
> The author suggests using **[michel/dotenv](https://github.com/michelphp/dotenv)** instead.


# PHP-DotEnv

PHP-DotEnv is a lightweight and robust PHP library designed to simplify the management of environment variables in your PHP applications. It parses a `.env` file and loads the variables into `getenv()`, `$_ENV`, and `$_SERVER`.

It comes with a powerful processor system that automatically converts values (booleans, nulls, numbers) and allows you to define your own custom processors for specific needs.

---

## English Documentation

### Installation

Requires PHP 7.4 or higher.

Install via Composer:

```bash
composer require phpdevcommunity/php-dotenv
```

### Basic Usage

1.  **Create a `.env` file** in your project root:

    ```dotenv
    APP_ENV=dev
    DATABASE_URL="mysql:host=localhost;dbname=test"
    DEBUG=true
    CACHE_TTL=3600
    API_KEY=null
    # This is a comment
    ```

2.  **Load the variables** in your PHP entry point (e.g., `index.php`):

    ```php
    <?php
    require 'vendor/autoload.php';

    use PhpDevCommunity\DotEnv;

    $envFile = __DIR__ . '/.env';

    // Load environment variables
    (new DotEnv($envFile))->load();

    // Access variables
    var_dump(getenv('APP_ENV')); // string(3) "dev"
    var_dump($_ENV['DEBUG']);    // bool(true)
    ```

### Features

#### 1. Automatic Type Conversion (Default Processors)
PHP-DotEnv automatically processes values using default processors:

-   **BooleanProcessor**: Converts `true` and `false` (case-insensitive) to PHP `bool`.
-   **NullProcessor**: Converts `null` (case-insensitive) to PHP `null`.
-   **NumberProcessor**: Converts numeric strings to `int` or `float`.
-   **QuotedProcessor**: Removes surrounding double quotes `"` or single quotes `'` from strings.

#### 2. Comments
Lines starting with `#` are treated as comments and ignored.

#### 3. Trimming
Keys and values are automatically trimmed of whitespace.

### Advanced Usage: Custom Processors

You can extend the functionality by creating custom processors. A processor must extend `PhpDevCommunity\Processor\AbstractProcessor`.

#### Creating a Custom Processor

Example: A processor that converts comma-separated strings into arrays.

```php
namespace App\Processor;

use PhpDevCommunity\Processor\AbstractProcessor;

class ArrayProcessor extends AbstractProcessor
{
    public function canBeProcessed(): bool
    {
        // Process only if value contains a comma
        return strpos($this->value, ',') !== false;
    }

    public function execute()
    {
        return array_map('trim', explode(',', $this->value));
    }
}
```

#### Registering Custom Processors

Pass an array of processor class names to the `DotEnv` constructor.
**Note:** When you pass custom processors, you must also include the default ones if you still want them to run.

```php
use PhpDevCommunity\DotEnv;
use PhpDevCommunity\Processor\BooleanProcessor;
use PhpDevCommunity\Processor\NullProcessor;
use PhpDevCommunity\Processor\NumberProcessor;
use PhpDevCommunity\Processor\QuotedProcessor;
use App\Processor\ArrayProcessor;

$processors = [
    BooleanProcessor::class,
    NullProcessor::class,
    NumberProcessor::class,
    QuotedProcessor::class,
    ArrayProcessor::class // Your custom processor
];

(new DotEnv(__DIR__ . '/.env', $processors))->load();
```

---

## 🇫🇷 Documentation Française

### Introduction

PHP-DotEnv est une bibliothèque PHP légère conçue pour simplifier la gestion des variables d'environnement dans vos applications PHP. Elle analyse un fichier `.env` et charge les variables dans `getenv()`, `$_ENV` et `$_SERVER`.

Elle intègre un système de processeurs qui convertit automatiquement les valeurs (booléens, null, nombres) et vous permet de définir vos propres processeurs personnalisés.

### Installation

Nécessite PHP 7.4 ou supérieur.

Installez via Composer :

```bash
composer require phpdevcommunity/php-dotenv
```

### Utilisation de Base

1.  **Créez un fichier `.env`** à la racine de votre projet :

    ```dotenv
    APP_ENV=dev
    DATABASE_URL="mysql:host=localhost;dbname=test"
    DEBUG=true
    CACHE_TTL=3600
    API_KEY=null
    # Ceci est un commentaire
    ```

2.  **Chargez les variables** dans votre point d'entrée PHP (ex: `index.php`) :

    ```php
    <?php
    require 'vendor/autoload.php';

    use PhpDevCommunity\DotEnv;

    $envFile = __DIR__ . '/.env';

    // Charger les variables d'environnement
    (new DotEnv($envFile))->load();

    // Accéder aux variables
    var_dump(getenv('APP_ENV')); // string(3) "dev"
    var_dump($_ENV['DEBUG']);    // bool(true)
    ```

### Fonctionnalités

#### 1. Conversion Automatique des Types (Processeurs par défaut)
PHP-DotEnv traite automatiquement les valeurs à l'aide de processeurs par défaut :

-   **BooleanProcessor** : Convertit `true` et `false` (insensible à la casse) en `bool` PHP.
-   **NullProcessor** : Convertit `null` (insensible à la casse) en `null` PHP.
-   **NumberProcessor** : Convertit les chaînes numériques en `int` ou `float`.
-   **QuotedProcessor** : Supprime les guillemets doubles `"` ou simples `'` entourant les chaînes.

#### 2. Commentaires
Les lignes commençant par `#` sont traitées comme des commentaires et ignorées.

#### 3. Nettoyage (Trimming)
Les espaces autour des clés et des valeurs sont automatiquement supprimés.

### Utilisation Avancée : Processeurs Personnalisés

Vous pouvez étendre les fonctionnalités en créant des processeurs personnalisés. Un processeur doit étendre `PhpDevCommunity\Processor\AbstractProcessor`.

#### Créer un Processeur Personnalisé

Exemple : Un processeur qui convertit les chaînes séparées par des virgules en tableaux.

```php
namespace App\Processor;

use PhpDevCommunity\Processor\AbstractProcessor;

class ArrayProcessor extends AbstractProcessor
{
    public function canBeProcessed(): bool
    {
        // Traiter uniquement si la valeur contient une virgule
        return strpos($this->value, ',') !== false;
    }

    public function execute()
    {
        return array_map('trim', explode(',', $this->value));
    }
}
```

#### Enregistrer les Processeurs Personnalisés

Passez un tableau de noms de classes de processeurs au constructeur de `DotEnv`.
**Note :** Lorsque vous passez des processeurs personnalisés, vous devez également inclure ceux par défaut si vous souhaitez qu'ils continuent de fonctionner.

```php
use PhpDevCommunity\DotEnv;
use PhpDevCommunity\Processor\BooleanProcessor;
use PhpDevCommunity\Processor\NullProcessor;
use PhpDevCommunity\Processor\NumberProcessor;
use PhpDevCommunity\Processor\QuotedProcessor;
use App\Processor\ArrayProcessor;

$processors = [
    BooleanProcessor::class,
    NullProcessor::class,
    NumberProcessor::class,
    QuotedProcessor::class,
    ArrayProcessor::class // Votre processeur personnalisé
];

(new DotEnv(__DIR__ . '/.env', $processors))->load();
```
