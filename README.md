# FormBuilder


### ModalForm
```php
use form_builder\models\modal_form_elements\ModalFormButton;
use form_builder\models\ModalForm;
use pocketmine\Player;

class ModalFormSample extends ModalForm
{
    public function __construct() {
        parent::__construct(
            "ModalFormSample",
            "Do you like apple?",
            new ModalFormButton("Yes"),
            new ModalFormButton("No"));
    }

    public function onClickButton1(Player $player): void {
        $player->sendMessage("Me too!");
    }

    public function onClickButton2(Player $player): void {
        $player->sendMessage("Why");
    }

    public function onClickCloseButton(Player $player): void {
        $player->sendMessage("Close");
    }
}
```

### SimpleForm
```php
use form_builder\models\simple_form_elements\SimpleFormButton;
use form_builder\models\simple_form_elements\SimpleFormImage;
use form_builder\models\simple_form_elements\SimpleFormImageType;
use form_builder\models\SimpleForm;
use pocketmine\Player;

class SimpleFormSample extends SimpleForm
{
    public function __construct() {
        parent::__construct("Sample", "This is sample",
            [
                new SimpleFormButton("Apple",
                    new SimpleFormImage(SimpleFormImageType::Path(), "textures/items/apple.png"),
                    function (Player $player) {
                        $player->sendMessage("Select Apple");
                    }
                ),
                new SimpleFormButton("Beef",
                    new SimpleFormImage(SimpleFormImageType::Url(), "https://~~/~~/beef.png"),
                    function (Player $player) {
                        $player->sendMessage("Select Beef");
                    }
                ),
                new SimpleFormButton("No Image",
                    null,
                    function (Player $player) {
                        $player->sendMessage("No Image");
                    }
                )
            ]);
    }

    function onClickCloseButton(Player $player): void {
        $player->sendMessage("Close The Form");
    }
}
```

### CustomForm

```php
use form_builder\models\custom_form_elements\Dropdown;
use form_builder\models\custom_form_elements\Input;
use form_builder\models\custom_form_elements\Label;
use form_builder\models\custom_form_elements\Slider;
use form_builder\models\custom_form_elements\StepSlider;
use form_builder\models\custom_form_elements\Toggle;
use form_builder\models\CustomForm;
use pocketmine\Player;
use pocketmine\Server;

class CustomFomSample extends CustomForm
{

    private $server;

    private $description;
    private $name;
    private $speciality;
    private $age;
    private $job;
    private $isPublic;

    public function __construct(Server $server, Player $player) {
        $this->server = $server;

        $this->description = new Label("?????????????????????????????????");
        $this->name = new Input("??????", "??????", $player->getName());
        $this->speciality = new Dropdown("??????", ["????????????", "??????????????????", "??????"]);
        $this->age = new Slider("??????", 0, 100, 30);
        $this->job = new StepSlider("??????", ["??????", "??????", "????????????", "??????", "??????", "?????????"], 0);
        $this->isPublic = new Toggle("???????????????????????????????????????????????????", false);

        parent::__construct("????????????",
            [
                $this->description,
                $this->speciality,
                $this->name,
                $this->age,
                $this->job,
                $this->isPublic,
            ]
        );
    }

    function onClickCloseButton(Player $player): void {
        // TODO: Implement onClickCloseButton() method.
    }

    function onSubmit(Player $player): void {
        $name = $this->name->getResult();
        $speciality = $this->speciality->getResult();
        $age = $this->age->getResult();
        $job = $this->job->getResult();
        $public = $this->isPublic->getResult();

        $text = "{$name}???{$age}?????????\n?????????{$speciality}??????????????????{$job}?????????";
        if ($public) {
            foreach ($this->server->getOnlinePlayers() as $onlinePlayer) {
                $onlinePlayer->sendMessage($text);
            }
        } else {
            $player->sendMessage($text);
        }
    }
}
```


### ?????????
```php
$player->sendForm(new ModalFormSample());
```