# altoiddealer's Discord Bot

### Uniting LLM ([text-generation-webui](https://github.com/oobabooga/text-generation-webui)) and Img Gen ([A1111](https://github.com/AUTOMATIC1111/stable-diffusion-webui) / [Forge](https://github.com/lllyasviel/stable-diffusion-webui-forge)) for chat & professional use.

- **text-generation-webui** is required. **Stable Diffusion** is optional.
- The features of both can be independently enabled/disabled in the main config file.

---

<img width="560" alt="Screenshot 2023-09-22 224716" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/5ffb5037-29d2-4a2f-9966-3ef6bd35b1f9">

---

### For support / discussion, visit the dedicated [ad_discordbot channel](https://discord.com/channels/1089972953506123937/1154970156108365944) on the TextGenWebUI Discord server.

---

**[What's new](#whats-new)   |   [Features](#features)   |   [Installation](#installation)   |   [Usage](#usage)   |   [Updating](#updating)**

---

## What's new:

<details>
  <summary>click to expand</summary>

   **05/28/2024:** Per-Channel History!

    - New setting 'per_channel_history' enables all channels to have their own chat history.
    - Custom logging format puts chat history, server name, and channel name under each channel.id key.
    - A utility .bat file is now included to split these custom logs into normal logs, if needed.
    - New '/announce/ command allows channels to be assigned as Announcement channels.
      Announcement channels will receive Model / Character change announcements
      instead of interaction channels.

  ---

   **05/22/2024:** Big Update - Easier to Update Moving Forward

   Quick shoutout to @Artificiangel who has recently joined development and made **stunning contributions**.

    - The directory '/internal/' which contains persistent settings (not intended to be modified by users)
      is no longer part of the bot package.  Instead, '/internal/' and its contents are created dynamically if missing.
    - User settings are now present in a '/settings_templates/' which will be automatically copied into the root directory,
      if not done manually by users.  This allows the bot to be easily updated without conflicts due to modified files.

  ---

   **05/16/2024:** Significant Changes to File Structure

    - The main bot script has grown massive, so it is now split to modules (new '/modules/' subdirectory)
    - activesettings.yaml is now in an '/internal/' subdirectory. Your settings will migrate automatically.
    - 'bot.db' has been superceded by a 'database.yaml' file. Your settings will migrate automatically.
    - Note: Changes to 'dict_base_settings.yaml' and 'activesettings.yaml' are just comment updates

  ---

   **04/29/2024:** Huge Improvement to ControlNet in /image Command

    - Options are now dynamically filtered and populated using the '/controlnet/control_type' API endpoint
    - Sets almost identical default options as in SD WebUI interface
    - This took a lot of time and effort, please try it out!

  <img width="690" alt="Screenshot 2024-04-29 145440" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/bd399667-8d1e-4dd8-9b36-074b9c4a3e54">

  ---

   **04/21/2024:** Revamped Main Config. Made textgenwebui and SD WebUI Optional!

    - replaced config.py with config.yaml
    - Config.py will still work, but is now unsupported and will receive no updates.
    - Textgenwebui and SD WebUI are now optional elements of the bot that can be disabled in config.yaml

  ---

   **04/19/2024:** Overhauled Img Model handling. Now "API" Method only.

  At first, there was only the '.YAML method' - each model required its own definition.

  Later, fetching models via API became a secondary option.

  Now, I noticed that all the improvements to API method have made the 'YAML method' obsolete:

    - Filter / Exclusion settings to control models that get loaded
    - Sophisticated calculations for 'Sizes' menu in '/image' command
    - Apply model settings and 'Tags' based on intelligent filter matching
    - Now, additional check for 'exact_match' if necessary.

  **Please take care migrating to this change**:

    - Fetch the new version of 'dict_imgmodels.yaml' which is now the settings panel for the API method.
    - Migrate your 'imgmodel' settings from config.yaml
    - Delete the whole 'imgmodels' block in config.yaml!

  ---


   **04/18/2024:** New Feature: Dynamic Prompting.

   Works ~~exactly~~ _**very similarly**_ to the SD WebUI extension [sd-dynamic-prompts](https://github.com/adieyal/sd-dynamic-prompts)

   **[Read up on it here!](https://github.com/altoiddealer/ad_discordbot/wiki/dynamic-prompting)**

  <img width="959" alt="Screenshot 2024-04-20 202457" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/6d1c0498-fc4d-4869-807e-904392ab44d2">

  ---


   **04/16/2024:** Enhanced Flows and "Instant Tags". Many other improvements.

    - Changed the 'Logging Level' from DEBUG to INFO - LESS SPAM!
    - Performance may be more optimized... all settings were being stored in the discord client object.
      Now, they are stored in a dedicated class object.
    - Characters can now be omitted from /character command with new parameter (see M1nty example char)
    - The feature to create tags instantly from your text has been upgraded.
      ANY tag values can be created including dictionaries, lists, sublists... anything.
    - The SD API "Guess imgmodel params" feature has much better success rate now.
    - "Flows" feature can now use variables for tag values.
    - Added a new "Flows" example in 'dict_tags.yaml'
    - Added new forge-couple param.
    - Better error handling when failure to change Img model
    - Fixed img prompt insertions from Tags sometimes creating a line break.

  <img width="1616" alt="Screenshot_2024-04-16_144136" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/141a03ae-0412-4cb0-a462-816c29eea857">

  ---

   **04/12/2024:** Changed user images dir. New Tags. Enhanced Image Selection.

     - All images now go into a root 'user_images' folder.
       There are no longer separate root folders for ReActor, ControlNet, Img2Img, Inpainting masks, etc.
       Users can organize their images in 'user_images' however they wish - just include path in Tags values.

     - New tags:
       - 'img2img'. Previous commit added img2img to /images cmd - now, it's also a Tag.
       - 'img2img_mask' (inpainting)...  and now also added to /image command!
       - 'send_user_image' - can send a local, non-AI generated image! Can be triggered to send an image after LLM Gen and/or after Img Gen.

    - Enhanced image selection:
      When processing images from tags (ReActor, ControlNet, etc etc), if the value is a directory (does not include .jpg/.png/.txt),
      the function will recursively attempt to select a random image - if no images are in the directory, it will try picking a random directory, and so on,
      until an image is found or reaches an empty directory (error).  So rather than just picking a random image from a folder, it can now pick from a random folder.

  <img width="1166" alt="example" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/1f4e1297-b69e-4c85-b525-d34c568d5477">

  ---

   **04/10/2024:** Upgraded '/image' cmd. Added Tags. Added [sd-forge-couple](https://github.com/Haoming02/sd-forge-couple) extension support.

    - Upgraded the /image command:

      - ControlNet and ReActor now only appear in the select options if enabled in config.yaml
      - 'img2img' has been added. If an image is attached, it will prompt for the Denoise Strength.
      - ControlNet now follows up asking for model/map if an image is attached, to simplify the main menu.

    - Added 'sd_output_dir' tag, so now you can control the image save location in your tag definitions.

    - Added extension support for SD Forge Couple.  Currently only useful for '/image' command unless you can get the LLM to reply with the correct format (I'm working on that!)

  <img width="658" alt="Screenshot_2024-04-10_140521" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/c5aa7146-92b5-43d2-87d7-8887320a45d8">

   ---

   **04/01/2024:** Pretty massive update. Be sure to update textgen-webui, and take care updating settings files.

    - Overhauled SD WebUI extension support (ControlNet, layerdiffuse, ReActor) to be much more powerful and manageable.

      - ControlNet support received a massive update in particular... multi-ControlNet is even supported!
      - These extensions each have a simple primary Tag to activate and apply.
      - **ALL** of their parameters are now easily controlled by the Tags system.

    - Added a new method to create Tags on-the-fly using this syntax: [[key:value]] or [[key1:value1 | key2:value2]]. These go into immediate effect, particularly useful for controlling the extension settings.

    - 4 older parameters were recently removed from textgen-webui - mirrored in this bot update.

   ---

   **03/07/2024:** [layerdiffuse](https://github.com/layerdiffusion/sd-forge-layerdiffuse) support added to Tags feature!

   <img width="603" alt="Screenshot 2024-03-07 104132" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/30975294-d8b4-4ae1-a53d-7f3616d8a22c">

   ---

   **02/13/2024:** Major update introducing the Tags feature. Take care migrating your existing settings

   <img width="1403" alt="Screenshot 2024-03-07 104231" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/89aae51f-2abe-43c2-bade-a2ea395be2da">

   ---

   **12/11/2023:** New "/speak" command! Silero and ElevenLabs TTS extensions now supported!

   <img width="595" alt="Screenshot 2024-03-07 104326" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/c7b42c39-c29f-4d69-af25-fff5a4d9dbcf">

   ---

   **12/8/2023:** TTS Support, and Character Specific Extension Settings now added!

   <img width="1152" alt="Screenshot 2024-03-07 104503" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/5bbd15bc-f181-4f49-bece-b633bdc7412d">

</details>

## Features:

- **Chat History for each channel!**
  - Each channel has its chat history maintained seperately and cleanly.
  - History is gracefully loaded for all channels on startup.
  - **`/reset_conversation`** will reset history in interaction channel only.

- **Robust ["Tags" system](https://github.com/altoiddealer/ad_discordbot/wiki/tags) to manipulate bot behavior persistently or via trigger phrases, including:**
  - Trigger Text and/or Image response
  - Image censoring settings (None / Spoiler / Block)
  - Powerful [ControlNet](https://github.com/Mikubill/sd-webui-controlnet), [ReActor](https://github.com/Gourieff/sd-webui-reactor), [Forge Couple](https://github.com/Haoming02/sd-forge-couple), and [layerdiffuse](https://github.com/layerdiffusion/sd-forge-layerdiffuse) integration
  - Automatically apply [loractl scaling](https://github.com/cheald/sd-webui-loractl) (Currently A1111 only)
  - Swapping / Changing LLM characters (ei: "draw... " can trigger character tailored for image prompting)
  - Swapping / Changing LLM models and Img models
  - Modifying LLM state (temperature, repetition penalty, etc) and image gen settings (width, height, cfg scale, etc)
  - Keep things spicy by factoring random variations to LLM state / img gen settings
  - Modifying the user's prompt and LLM's reply with delete/insert/replace text, img LORAs, etc
  - Manipulate LLM history (Suppress history, limit history, save or do-no-save reply, etc)
  - **New Feature:** Flexible, Instant-Tags by using syntax [[key:value]] or [[key1:value1 | key2:value2 ]]
  - New features being added frequently and easily due to the framework of the ["Tags" system](https://github.com/altoiddealer/ad_discordbot/wiki/tags)

- **TTS Support!**
  - [alltalk_tts](https://github.com/erew123/alltalk_tts), coqui_tts, silero_tts, and elevenlabs_tts
  - Bot can speak on Voice channel, upload copy of audio file, or both!
  - Per-character TTS settings! Give each character a unique voice!

- **Sophisticated function to send text responses over Discord's 2,000 character limit**
  - "chunks" messages by looking back to nearest line break or sentence completion.
  - Preserves syntax between chunks such as **bold**, *italic*, and even `code formatting`

- **Commands!**
  - **/helpmenu** - Display information message
  - **/character** - Change character
  - **/main** - Toggle if Bot always replies, per channel
  - **/announce** - Toggle channels as "Announcement" channels
  - **/image** - Allows more controlled image prompting (positive prompt, neg prompt, Size settings, **ControlNet**, **ReActor**)
  - **/speak** - Bot can speak any text, using any voices (including user attach .mp3 or .wav for alltalk_tts)!
  - **/imgmodel** - Change A1111 model & img_payload settings
  - **/llmmodel** - Change LLM model

- **Dynamic settings handling:**
  - Core bot settings managed in **`config.yaml`** (bot behavior, discord features, extensions, etc.)
  - The ["Tags" system](https://github.com/altoiddealer/ad_discordbot/wiki/tags) is configured in **`dict_tags.yaml`** (global Tags, default Tag params, Tag presets, etc.)
  - Foundational layer of user settings configured in **`base_settings.yaml`**.
  - Character files can include custom Tags, TTS settings, LLM state parameters, and special behaviors, which prioritize over basesettings.
  - Custom Image models settings defined in **`dict_imgmodels.yaml`** (Tags, payload params) which prioritize over basesettings.
  - All user settings commit to **`internal/activesettings.yaml`**, which serves as a dashboard to view the bot's current state.

- **Automatic Img model changing:**
  - Adjustable duration and mode (random / cycle)
  - Smart filters and settings to auto-update relavent settings (SD1.5, SDXL, Turbo, etc)

- **Continue and Regenerate text replies via Context Menu (right click on the reply)**

- **All tasks queue up and process elagently - go ahead, spam it with requests!**

- **Built in Starboard feature**

- **Feature to post current settings in a dedicated channel**

- **ALWAYS MORE TO COME**

---

<img width="817" alt="Screenshot 2023-09-22 220802" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/b2f2bd96-ed12-4eed-b842-68171c62a8e5">

---

## Installation

1. **Install oobabooga's [text-generation-webui](https://github.com/oobabooga/text-generation-webui)**

2. **[Create a Discord bot account](https://discordpy.readthedocs.io/en/stable/discord.html), invite it to your server, and note its authentication token**.

3. Clone this repository into **/text-generation-webui/**
   ```
   git clone https://github.com/altoiddealer/ad_discordbot
   ```

4. Move **bot.py** out of **/ad_discordbot/** -> into **/text-generation-webui/**

   `/text-generation-webui/bot.py`

   `/text-generation-webui/ad_discordbot/(remaining files)`

5. **Run the .cmd file** in text-generation-webui directory (**ex: cmd_windows.bat**), and performing the following commands:
   ```
   pip install -r ad_discordbot\requirements.txt
   ```

6. **Enter your bot token (from Step 2) into the CMD window**

   <img width="559" alt="Screenshot 2024-05-25 100216" src="https://github.com/altoiddealer/ad_discordbot/assets/1613484/ff5207ec-954e-4e71-975d-ce171c1c42c8">

8. **The bot should now be up and running!**
  
   If a Welcome message does not appear in your main channel, you can use `/helpmenu` to see it.

   A number of user settings files will appear (copied in from **`/user_settings/`**) where you can customize the bot.

   `config.yaml` , `dict_base_settings.yaml` , `dict_cmdoptions.yaml` , `dict_imgmodels.yaml` , `dict_tags.yaml`

---

### Running the bot

1. **Run the .cmd file** in text-generation-webui directory (**ex: cmd_windows.bat**)
   ```
   python bot.py (args)
   ```

   **EXAMPLE LAUNCH COMMAND:**
   ```
   python bot.py --loader exllama --model airoboros-l2-13b-gpt4-2.0-GPTQ
   ```
2. Use [command](https://github.com/altoiddealer/ad_discordbot/wiki/commands) **/character** to choose a character.

---

## Usage:

### Getting responses from the bot:

* @ mention the bot

* Use [command](https://github.com/altoiddealer/ad_discordbot/wiki/commands) **/main** to set a main channel. **The bot won't need to be @ mentioned in main channels.**

* If you enclose your text in parenthesis (like this), the bot will not respond.

### Getting image responses from the bot

(**A1111 or sd-webui-forge must be running!**)

* By default, starting your request with "draw " or "generate " will trigger an image response via the Tags system (see **`dict_tags.yaml`**)

* Use **/image** command to use your own prompt with advanced options

### Getting TTS responses from the bot (Tested: alltalk_tts, coqui_tts, silero_tts, elevenlabs_tts)

1. **Run the .cmd file** in text-generation-webui directory (**ex: cmd_windows.bat**), and performing the following commands:

   Required for bot to join a discord voice channel:
   ```
   pip install pynacl
   ```

2. **Install your TTS extension**.

   **Follow the specific instructions for your TTS extension!!**

   Example instructions for **coqui_tts**:

   **Run the .cmd file** in text-generation-webui directory (**ex: cmd_windows.bat**), and performing the following commands:

   Linux / Mac:
   ```
   pip install -r extensions/coqui_tts/requirements.txt
   ```

   Windows:
   ```
   pip install -r extensions\coqui_tts\requirements.txt
   ```

5. Ensure that your bot has sufficient permissions to use the Voice channel and/or upload files (From your bot invite/Discord Developer portal, and your Discord server/channel settings)

6. Configure **config.yaml** in the section **discord** > **tts_settings**

7. If necessary, model file(s) should download on first launch of the bot.  If not, then first launch textgen-webui normally and enable the extension.

8. **Your characters can have their own settings including voices!  See example character M1nty for usage**

---

## Updating

1. **Open a cmd window** in **/ad_discordbot/** and **git pull**
   ```
   git pull
   ```

2. **IF bot.py has changed:**

   Copy and Replace **bot.py** -> into the directory **/text-generation-webui/** (*overwriting old version*)

   `.../text-generation-webui/bot.py`

   /text-generation-webui/ad_discordbot/(remaining files)

3. **IF other files have changed ('config.yaml', 'dict_X.yaml', etc:**

   **The bot should continue functioning, but you may miss out on new features until migrating new settings to your existing settings**
   - migrate the changes from the files in **`settings_templates`** into your settings,
   - **OR** migrate values from your settings into a copy of the updated templates

   **Example**: config.yaml gets updated with a new feature.

   Solution is to either:

   * Update your existing config.yaml with the new feature,

   * **OR** make a copy of the new config.yaml and update it with your settings.
