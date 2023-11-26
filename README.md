# Golang Telegram Bot API

## WHY???

☠️ The original repo `https://github.com/go-telegram-bot-api/telegram-bot-api` has been __abandoned since December 2021
__ and has fallen far behind in supporting the Telegram API.

🥴 Actual fork `https://github.com/OvyFlash/telegram-bot-api` does not have tags and is not available for `go get`

## Install

```console
go get -u github.com/antikirra/telegram-bot-api
```

## Usage

```go
package main

import (
	"log"

	tgbotapi "github.com/antikirra/telegram-bot-api"
)

func main() {
	bot, err := tgbotapi.NewBotAPI("MyAwesomeBotToken")
	if err != nil {
		log.Panic(err)
	}

	bot.Debug = true

	log.Printf("Authorized on account %s", bot.Self.UserName)

	u := tgbotapi.NewUpdate(0)
	u.Timeout = 60

	updates := bot.GetUpdatesChan(u)

	for update := range updates {
		if update.Message != nil { // If we got a message
			log.Printf("[%s] %s", update.Message.From.UserName, update.Message.Text)

			msg := tgbotapi.NewMessage(update.Message.Chat.ID, update.Message.Text)
			msg.ReplyToMessageID = update.Message.MessageID

			bot.Send(msg)
		}
	}
}
```