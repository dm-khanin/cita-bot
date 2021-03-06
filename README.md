Cita helper
===========

This selenium automatization python script helps to catch cita timeslot.

**It DOES make a reservation (semi)automatically**

...if you set up a Telegram bot and use it for SMS confirmation. Otherwise:

Enable your speakers and wait for "ALARM ALARM ALARM" message :) Next you'll have to select timeslot and confirm an appointment via SMS code.

Installation TL;DR
-------------------

1. Install Python 3.8: https://www.python.org/downloads/release/python-385/

2. `pip install -r requirements.txt`

3. Install Google Chrome.

4. Download [chromedriver](https://chromedriver.chromium.org/downloads) and put it in the PATH (Python dir from step 1 should work).

5. Get plugin https://antcpt.com/downloads/anticaptcha/chrome/anticaptcha-plugin_v0.50.crx

6. Get API Key from https://anti-captcha.com ($5 is enough, trust me! :)

7. Copy example file and fill your data, save it as `grab_me.py`.

8. Run `python grab_me.py`, follow the voice instructions.


Examples
--------

* `example1.py` — Recogida de tarjeta

* `example2.py` — Toma de huellas

* `example3.py` — Solicitud de autorizaciones


Options
--------

```
@dataclass
class CustomerProfile:
    anticaptcha_api_key: Optional[str] = None
    anticaptcha_plugin_path: Optional[str] = None
    auto_captcha: bool = True
    auto_office: bool = True
    chrome_driver_path: str = None
    chrome_profile_name: Optional[str] = None
    chrome_profile_path: Optional[str] = None
    fast_forward_url: Optional[str] = None
    save_artifacts: bool = False
    telegram_token: Optional[str] = None
    wait_exact_time: Optional[list] = None # [[minute, second]]

    city: str = "Barcelona"
    operation_code: OperationType = OperationType.TOMA_HUELLAS
    doc_type: DocType
    doc_value: str  # Passport? "123123123"; Nie? "Y1111111M"
    name: str
    country: str = "RUSIA"
    year_of_birth: Optional[str] = None
    card_expire_date: Optional[str] = None  # "dd/mm/yyyy"
    phone: str
    email: str
    offices: Optional[list] = field(default_factory=list)
```

* `anticaptcha_api_key` — Anti-captcha.com API KEY (not required if auto_captcha=False)

* `anticaptcha_plugin_path` — Full path for the plugin file.

* `auto_captcha` — Should we use Anti-Captcha plugin? For a testing purposes, you can disable it and solve reCaptcha by youreslf. Do not click "Enter" or "Accept" buttons, just solve captcha and click Enter in the Terminal.

* `auto_office` — Automatically choose of the police station. If `False`, again, select an option in the browser manually, do not click "Accept" or "Enter", just click Enter in the Terminal.

* `telegram_token` — Telegram bot token for SMS confirmation. Wait for SMS and confirm appointments with a command `/code 12345`

* `wait_exact_time` — Set specific time (minute and second) you want it to hit `Solicitar cita` button

* `city` — City name (Barcelona by default). Copypaste using Chrome DevTools from the Select options, please.

* `operation_code` — `OperationType.TOMA_HUELLAS` or `OperationType.RECOGIDA_DE_TARJETA` or `OperationType.SOLICITUD`

* `doc_type` — `DocType.NIE` or `DocType.PASSPORT`

* `doc_value` — Document number, no spaces

* `name` — First and Last Name

* `year_of_birth` — Year of birth, like "YYYY"

* `country` — Country (RUSIA by default). The same copypaste, please.

* `card_expire_date` — Card Expiration Date. Probably, it's not important at all, leave it empty.

* `phone` — Phone number, no spaces, like "600000000"

* `email` — Email

* `offices` — Required field for for RECOGIDA_DE_TARJETA! If provided, script will try to select the specific police station or end the cycle. For TOMA_HUELLAS it attempts to select all provided offices one by one, otherwise selects a random one.

**Chrome Profile Persistence**

It should be easier to resolve captcha if you use Chorme Profile with some history, so it's better to preserve browser history between attempts.

```python
        # chrome_profile_path=f"{os.curdir}/chrome_profiles/",  # You can persist Chrome profile between runs, it's good for captcha :)
        # chrome_profile_name="Profile 7",  # Profile name
```

Try to uncomment these lines in the run script.


How to fix dependencies
------------------------

1. For Windows, download [wsay](https://github.com/p-groarke/wsay/releases) and put it in the PATH (see step 4 of Installation)

3. Chrome → Firefox — it's possible as well (tune code, paths, browser run arguments, plugin)
