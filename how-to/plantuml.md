# PlantUML

Main documentation: [https://plantuml.com](https://plantuml.com)

## Usage

As I don't like to have a JVM on my local computer, I use a docker image to render an image:

```bash
input=${1:?"input file path is required"};
output=${2:-"${input:r}.png"};

docker run \
  --rm \
  -i think/plantuml \
  -tpng \
  < "${input}" \
  > "${output}"
```

## Black & White for the win

```text
skinparam NoteBackgroundColor #fff
skinparam NoteBorderColor #000

skinparam sequence {
    ArrowColor #000
    ActorBorderColor #000

    LifeLineBorderColor #000
    LifeLineBackgroundColor #000

    ParticipantBorderColor #000
    ParticipantBackgroundColor #fff
    ParticipantFontColor #000

    ActorBackgroundColor #000
    ActorFontColor #000
}
```

## Sequence Diagram reminders

* Create a human actor:

  ```text
  actor autocompleteFriendlyName as "Human friendly name"
  ```

* Create a machine:

  ```text
  participant autocompleteFriendlyName as "Human friendly name"
  ```

* Classical sequence arrow:

  ```text
  autocompleteFriendlyName -> anotherAutocompleteFriendlyName : A description of what's happening
  ```

* Less prominent arrow (dotted + italic):

  ```text
  autocompleteFriendlyName -->> anotherAutocompleteFriendlyName : //A description of what's happening//
  ```

* Numbering:
  * Start numbering the sequence: `autonumber`
  * Pause the numbering: `autonumber stop`
  * Resume the numbering: `autonumber resume`
* Create a delay:

  ```text
  ... waiting for something to happen ...
  ```
