---
layout: doc
date: 2023-11-06
title: Upload a file with HTTParty without using a temporary file
tags:
- ruby
- httparty
---

<Title />

When you need to upload a file to HTTP POST endpoint using form encoding and you want to use HTTParty to upload data that you have as a string you have two options:

While you could write the string to a temp file and upload the result it feels a lot cleaner to write the string contents directly.

This is a little more involved and it goes like this:

```ruby
require 'httparty'

module YourApiWrapper
  class Base
    include HTTParty

    base_uri 'https://examples.com:/api/v1'
    headers({
              accept: 'application/json',
              authorization: "Bearer: #{ENV['YOUR_AUTH_TOKEN']}"
            })
  end

  class File < Base
    def self.attach(some_id:, file_name:, file_contents:)
      file_to_upload = StringIO.new file_contents

      def file_to_upload.path=(path)
        @path = path
      end

      def file_to_upload.path
        @path
      end

      file_to_upload.path = file_name

      file_to_upload.rewind

      post(
        '/files',
        multipart: true,
        body: {
          file: file_to_upload,
          some_id: some_id
        }
      )
    end
  end
end
```

<Comment />
