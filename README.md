# Voice support tools and scripts for Music Assistant

Bridge the gap between Home Assistant Assist and Music Assistant for voice-initiated music control.

This repository is meant to be an effort to collect scripts, blueprints, HOWto's etc. and their translations to enable full Voice control of music playback.
Home Assistant itself has very limited intents for music/media control, and especially there is no support for initiating playback of music by voice.
While this support is on the horizon to be added, there is still a lot to investigate about that to create a universal way to provide that support in HA natively. Maybe the resources/experiments from this repository can help make that journey easier in the future.

**Community driven effort!**
Please reach out if you want to help out or have great ideas!
If you have something to share here, feel free to PR it or ask to be added to the maintainers.

## Option 1: Local Assistant Blueprint

Blueprint to initiate media playback using pure local intent handling (custom sentences) so no Cloud-LLM involved.
This however means that requests need to be very carefuly formulated.

The Blueprint is located in the `local-assist-blueprint` folder of this repository and can be imported by using the following buttons:

* English: [![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Flocal-assist-blueprint%2Fmass_assist_blueprint_en.yaml)
* German: [![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Flocal-assist-blueprint%2Fmass_assist_blueprint_de.yaml)
### Usage

All sentences must:

- start with the words `Play` or `Listen to` followed by the item type `artist/track/album/playlist/radio station` and then the name of the item

- for album and track be optionally followed by `by (the) artist` and then the artist name

- then be optionally followed by an area name or device name

- then, for artist, track, album or playlist, be optionally followed by the phrase `using radio mode`

#### Acceptable variations

There are acceptable variations to some words and inclusion of the word `the` is optional.

#### Examples

```

Play the artist Pink Floyd in the kitchen

Listen to album Jagged Little Pill in the study

Listen to the album Greatest Hits by the artist James Taylor in the kitchen

Play track New Years Day in the bedroom

Play track New Years Day in the bedroom using radio mode

Play the song A Hard Days Night by Billy Joel in the bedroom

Listen to the playlist Classic Rock in the study

Listen to the radio station BBC Radio 1 in the bedroom

Play the album Classical Nights on the Bedroom Sonos Speaker

Listen to the record Classical Nights on the Bedroom Sonos Speaker

Play the band U2
```

## Option 2

Script which can be used by an LLM integration like [Open AI Conversation](https://www.home-assistant.io/integrations/openai_conversation/) (ChatGPT) or [Google Generative AI](https://www.home-assistant.io/integrations/google_generative_ai_conversation/) (Gemini).

The blueprint is located in the `llm-script-blueprint` folder of this repository and can be imported by using the following button:

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmusic-assistant%2Fvoice-support%2Fblob%2Fmain%2Fllm-script-blueprint%2Fllm_voice_script.yaml)

The language of the voice command is not relevant, the script has all descriptions in English, but it will be used by voice commands issued in a different language as well.

### Configuration

1. An LLM integration needs to be set up, and used in your Voice pipeline
2. Allow the LLM integration to access your house, otherwise it won't be able to use the script as a tool
3. Import the Blueprint using the button above
4. Create a script using the blueprint, optionally you can adjust the prompt settings which are the basis on how the script will be used by the LLM
5. Save the script, make sure to give it a clear description, as that is what will be used by the LLM to determine when to use it. 
6. Expose the script to Assist

Suggestion for the script description:
>   This script is used to play music based on a voice request. The tool takes the following arguments: media_type, artist, album, media_id, radio_mode, area. media_id, media_type, and area are always required and must always be supplied as arguments to this tool. Use the parameters as described in the description of each parameter. Use this tool whenever the user requests to play music.

### Usage

There is no required format for the sentences, just use anything you can imagine to play music. You can add an area in which it should be played. If the area is ommited from your voice request, it will take the area from which the command is issued.

It will differ per LLM integration how well the commands are understood. If the command is not clear enough, the LLM might ask for more details. Escpecially for smaller models more guidance can be required. If needed you can adjust the prompts used for each parameter.

### Examples
All responses and results are generated each time the script is used, so don't expect the exact same results as below.

- Command: *Play the album from that grunge band with the baby swimmng towards a bank note on the album cover in the room in which we prepare our meals*
  
  Response:  *The album "Nevermind" by Nirvana is now playing in the area in which meals are prepared*
  
  Result: The album "Nevermind" will be played in the kitchen

- Command: *Play some classic rock songs*
  
  Response: *I've started playing some classic rock songs in the office. Enjoy the music!*

  Result: The following 5 songs are played
    * Led Zeppelin - Stairway to Heaven
    * Queen - Bohemian Rhapsody
    * The Rolling Stones - Paint It Black
    * The Who - My Generation
    * AC/DC - Back in Black
