---
title: Improve Ansible Roles with Molecule 
separator: <!--s-->
verticalSeparator: <!--v-->
theme: night

revealOptions:
    transition: 'slide'
    backgroundTransition: 'fade'
    viewDistance: 3
    slideNumber: true
    hideAddressBar: true
    parallaxBackgroundImage": "assets/img/Background1.jpg"
    parallaxBackgroundSize": "2100px 900px"
    parallaxBackgroundHorizontal": 200
    parallaxBackgroundVertical": 50
    width: "100%"
	  height: "110%"
	  margin: 0
	  minScale: 1
	  maxScale: 1
---

##  Improve Ansible Roles with Molecule 
## (Infrastructure Testing with Molecule)

<!--s-->
## Luis Cacho

### Linux Administrator [@Rackspace](https://rackspace.com)

<br/>
[luiscachog.io](https://luiscachog.io) &bull;
[@k4ch0](https://twitter.com/k4ch0) &bull;
[github.com/k4ch0](https://github.com/k4ch0)

<!--s-->
## Agenda

- Ansible Review
  - YAML Review
- Test Automation
  - Testing options for Ansible
- Molecule
- Demo!!

<!--s-->

<!-- .slide: data-background="assets/img/BackgroundAnsible.png" -->
<!--v-->

## Ansible

<section>
    <div class='multiCol'>
        <div class='col'>
            Gallia est omnis divisa in partes tres, quarum unam incolunt Belgae, aliam Aquitani, tertiam qui ipsorum lingua Celtae, nostra Galli appellantur.
        </div>
        <div class='col'>
            Qua de causa Helvetii quoque reliquos Gallos virtute praecedunt, quod fere cotidianis proeliis cum Germanis contendunt, cum aut suis finibus eos prohibent aut ipsi in eorum finibus bellum gerunt.
        </div>
    </div>
    And simply more regular full-width text in the following. But hey, there is also:
    <div class='multiCol'>
        <div class='col'>Also works for 3 columns...</div>
        <div class='col'>...as we can show in...</div>
        <div class='col'>...this example here.</div>
    </div>
</section>


<!--v-->

## Ansible

<section>
  <div style="text-align: left; float: left;">
    **Attributes**</p>
    - Simple</p>
    - Powerful</p>
    - Agentless</p>
    - Cross Platform</p>
    - Over 450 Modules</p>
    - Community</p>
    <!-- more Elements -->
  </div>

  <div style="text-align: left; float: right;">
    **Use Cases**</p>
    - Configuration Management</p>
    - Security and Compliance</p>
    - Application Deployment</p>
    - Orchestration</p>
    - Continuous Delivery</p>
    - Provisioning</p>
    <!-- more Elements -->
  </div>
</section>

Note:
Ansible es una herramienta de automatización open source escrita en python, que nos ayuda a configurar hosts remotos, aprovisionar software, e inclusive orquestar tareas mas complicadas como Continuos Deployment o Rolling Updates con Cero Downtime,
Instead of writing custom, individualized scripts, system administrators create high-level plays in Ansible. 

<!--v-->

## Ansible

![Ansible](assets/img/Ansible_all_in_one.png)

Note:
Continuando con el slide anterior, tenemos algunos casos de uso y la alternativa a ansible.

<!--v-->

## Ansible

![Ansible Architecture](assets/img/AnsibleArch.png)

Note:
Revisando la arquitectura de Ansible tenemos:

- Playbook: "El libro de jugadas" que contiene las tareas a ejecutar.
- Inventario:donde se definen los hosts con los cuales va a interactuar ansible, esta formato INI. Se definen grupos de hosts y algunas variables opcionales ()IP, hostname, ssh user, key file)
- Modulos son pequenos programas que ejecuta Ansible de manera remota.
- API: Aun esta en construccion pero hay plugins que ya la usan (ansible tower)
- Plugins: Agregan funcionabilidad adicional al core de Ansible (ARA)

Todo esto se se aplica a los hosts remotos o a los dispositivos de red.

<!--v-->

## Ansible

<img src="assets/img/Ansible_Playbook.png" width="70%" height="70%" >

Note:
Los componentes de playbook son, revisando desde la unidad mas pequena son:

- Tasks, que ejecutan el modulo de Ansible
- Al conjunto de varias tasks relacionadas, se le llama Play
- Al conjunto de diferentes Plays se le llama Playbook.

<!--v-->

## Ansible

- **Playbooks** contain **plays**  
- **Plays** contain **tasks**
- **Tasks** execute a **module**
- **Tasks** run sequencially
- **Handlers** are triggered by **tasks**, runs once at the end of the **play**

Note:
Revisando desde la unidad mas grande tenemos:
Cabe destacar que un Role se considera un playbook

<!--s-->

<!-- .slide: data-background="assets/img/BackgroundYaml.png" -->

<!--v-->

## YAML
#### YAML Ain't Markup Language

- Human-readable
- Machine-parseable language
- Data hierachy structures are maintained using identation.
- Data serialization format
  - JSON
  - XML
  - CSV

Note:
Legible para los humanos
Es un lenguaje que la maquina lo puede analizar facilmente
Las estructuras de datos de mantienen usando identacion (Python)
Tiene un forato de serialización de datos, ejemplos de ello son: JSON, XML, CSV
Serialization Format: A language used to convert or represent structured data or objects as series of characteres that can be stored on a disk.

<!--v-->

## YAML
#### YAML Ain't Markup Language

<section>
  <div style="text-align: left; float: left; font-size: 34px">
    - Documents begin with `---` </p>
      and end with `...` </p>
    - Identation denotes structure </p>
    - Comments begin with `#` </p>
    - Member of lists begin with `-` </p>
    - Key-value par use: </p>
      `<key>:<value>` </p>
    <!-- more Elements -->
  </div>

  <div style="text-align: right; float: right;" >
    <img style="float: right;" src="assets/img/YAML_Syntax.png" width="80%" height="80%" > <!-- .element: class="fragment" -->
  </div>
</section>

Note:
Los archivos empiezan con 3 guiones seguidos y terminan con 3 puntos (opcionales)
La identación marca estructura del archivo
Los comentarios comienzan con "gatito' o "hash"
Los miembros de una lista comienzan con un guion
Los valores de llave-valor se denotan como...

<!--s-->

<!-- .slide: data-background="assets/img/BackgroundTest.png" -->

<!--v-->

## Test Automation

- Reliable Code
- Quality
  - Fast feedback
- Time and cost saving
- Faster Development Cycle
  - CI/CD
- Repeatability
  - Test same change accross multiple environments (OS, Providers); multiple data sets

Note:
La automatizacion de las pruebas nos ayuda a:

- Tener un codigo mas seguro y confiable
- Mejora la calidad de nuestro codigo ya que permiten que se ejecuten más pruebas en menos tiempo, aumentando la cobertura de las pruebas, y realizando más exigentes.
- Reducimos el tiempo y el costo de un desarrollo
- Nos ayuda a tener un ciclo de desarrollo mas rapido ya que lo podemos/debemos integrar con alguna herramienta de CI/CD
- Repetibilidad, con lo cual podemos probar el mismo cambio en multples ambientes, sistemas operativos, o provides, o con diferentes data sets

<!--v-->

## Test Automation
#### Testing options for Ansible

- Ansible tasks *- Test Ansible w/ Ansible*<!-- .element: class="fragment" -->
- Test Kitchen *- Test Ansible w/ Ruby*<!-- .element: class="fragment" -->
- ansible-test *- Test Ansible w/ Unmaintained Python*<!-- .element: class="fragment" -->
- Molecule *- Test Ansible w/ Python*<!-- .element: class="fragment" -->

Note:
Algunas herramientas que revise, despues de ver la conferencia de Elana Hashman en en Ansible Fest 2017 son:

Testing Ansible with Ansible Tasks -> Test ansible with Ansible tasks
Benefits:

- As flexible and powerful as you need it
Issues:
- High development cost
- Ansible can't detect ansible bugs
- Need to write your own provisioner

Testing Ansible with ansible-test -> 
Benefits:

- Written in Python
- Solves the provisioning problem in "Test ansible with Ansible"
- Simple tool with small codebase
Issues:
- Onlys support docker provisioner on dEbian-based images
- Does not apear to be actively maintained

Testing Ansible with Test Kitchen -> 
Benefits:

- Large community
- Supports Ansible
- Supportw windows testing for Ansible
Issues:
- Written in Ruby
- Verifiers are Ruby or bash based
- Installs Ansible on the target host and runs it locally

<!--s-->

<!-- .slide: data-background="assets/img/BackgroundMolecule.png" -->

<!--v-->

## Molecule
#### Testing Ansible with Molecule

- Is designed to aid in the development and testing of Ansible roles.
- Provides support for testing with multiple instances, operating systems and distributions, virtualization providers, test frameworks and testing scenarios.
- Encourages an approach that results in consistently developed roles that are well-written, easily understood and maintained.

[https://github.com/ansible/molecule](https://github.com/ansible/molecule)
</br>
[https://molecule.readthedocs.io/](https://molecule.readthedocs.io/)

Note:


<!--v-->

## Molecule
#### Testing Ansible with Molecule

<section>
  <div style="text-align: left; float: left;">
    **Pros**</p> 
    - Written in Python</p>
    - Ansible-Native </p>
    - Large community</p>
    - Support for Python-based </br> verification with testinfra</p>
  </div>

  <div style="text-align: left; float: right;">
    **Cons**</p>
    - No Windows Support </p>
    - No Dinamyc Inventory Support</p>
  </div>
</section>

<!--v-->

## Molecule
#### Testing Ansible with Molecule

<section>
  <div style="text-align: left; float: left;">
    **Drivers**</p>
    - Docker (default)</p>
    - Vagrant</p>
    - Openstack</p>
    - More...</p>
    <!-- more Elements -->
  </div>

  <div style="text-align: left; float: right;">
    **Verifiers**</p>
    - Testinfra (default)</p>
    - Goss</p>
    - Inspect</p>
    <!-- more Elements -->
  </div>
</section>

<!--v-->

## Molecule
#### Testing Ansible with Molecule
- What can I test?
  - Files exists and permissions
  - Service are running
  - User exists and is member of the correct groups
  - Package installed
  - Basic Software interaction (Test web server basic authentication)

<!--v-->

## Molecule
#### Testing Ansible with Molecule

- Creates nodes for testing 
- Run the playbook on the nodes
- Run the playbook again to test idempotence
- Lints the Ansible code with ansible-lint
- Lint the Python code with flake8
- Runs the verifier tests on the nodes to ensure the desired state

<!--v-->

## Molecule
#### Subcommands

| Command | Description |
------- | ------- |
| check       | Use the provisioner to perform a Dry-Run... |
| converge    | Use the provisioner to configure instances... |
| create      | Use the provisioner to start the instances. |
| dependency  | Manage the role's dependencies. |
| destroy     | Use the provisioner to destroy the instances. |
| idempotence | Use the provisioner to configure the... |
| init        | Initialize a new role or scenario. |
| lint        | Lint the role. |

<!--v-->

## Molecule
#### Subcommands (cont'd)

| Command | Description |
------- | ------- |
| list        | Lists status of instances. |
| login       | Log in to one instance. |
| matrix      | List matrix of steps used to test instances. |
| prepare     | Use the provisioner to prepare the instances... |
| side-effect | Use the provisioner to perform side-effects... |
| syntax      | Use the provisioner to syntax check the role. |
| test        | Test (lint, destroy, dependency, syntax,... |
| verify      | Run automated tests against instances. |

<!--s-->

<!-- .slide: data-background="assets/img/Background1.jpg" -->

<!--v-->

## Molecule Demo
- Let's try!! <!-- .element: class="fragment" -->
  - Creates 2 nodes <!-- .element: class="fragment" -->
  - Converge both nodes <!-- .element: class="fragment" -->
  - Check for idempotence <!-- .element: class="fragment" -->
  - Lint the Ansible and Python code <!-- .element: class="fragment" -->
  - Verify the role against some tests <!-- .element: class="fragment" -->
</br>
- Github Repo: <!-- .element: class="fragment" --> *(Thanks to Garaged for provide a playbook to test)*<!-- .element: class="fragment" -->
</br>[https://github.com/k4ch0/elastic_stack](https://github.com/k4ch0/elastic_stack) <!-- .element: class="fragment" -->

<!--v-->

## Molecule Demo

```shell
$ molecule init role --role-name elastic_stack
--> Initializing new role elastic_stack...
Initialized role in /home/luis7238/elastic_stack/ successfully.
```
<!-- .element: class="fragment" -->

<!--v-->

## Molecule Demo

### Terminal time!! <!-- .element: class="fragment" -->

<!--v-->

## Molecule Demo
#### TO-DO

- Ansible-Vault implementation <!-- .element: class="fragment" -->
- Integrating Molecule into Travis CI, Circle CI, Jenkinks, etc <!-- .element: class="fragment" -->

<!--s-->

<!-- .slide: data-background="assets/img/BackgroundConclusion.jpg" -->

<!--v-->

## Conclusion <!-- .element: class="fragment" -->

- There are different testing solutions for Ansible, but Molecule is an Ansible-native and the robust option <!-- .element: class="fragment" -->
- Molecule allows you to create, converge, check idempotence, lint and verify your Ansible code <!-- .element: class="fragment" -->
- Molecule help you out to create the best playbooks possible <!-- .element: class="fragment" -->

<!--s-->

## Questions? <!-- .element: class="fragment" -->

<!--s-->

## Thank you! <!-- .element: class="fragment" data-fragment-index="1" -->

*Talk links, references and resources can be found at:* <!-- .element: class="fragment" data-fragment-index="2" -->
</br>
[https://luiscachog.io/talks/molecule-2019-03-01](https://luiscachog.io/talks/molecule-2019-03-01) <!-- .element: class="fragment" data-fragment-index="3" -->
