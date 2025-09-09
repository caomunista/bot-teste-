    case "ytmp3": {
      if (!args[0]) {
        return sock.sendMessage(from, { text: "❌ Envie o link do YouTube.\nExemplo: !ytmp3 https://youtu.be/xyz" }, { quoted: msg });
      }

      const url = args[0];
      if (!ytdl.validateURL(url)) {
        return sock.sendMessage(from, { text: "❌ Link inválido." }, { quoted: msg });
      }

      try {
        const info = await ytdl.getInfo(url);
        const title = info.videoDetails.title;

        const stream = ytdl(url, { filter: "audioonly", quality: "highestaudio" });
        let buffer = [];
        stream.on("data", chunk => buffer.push(chunk));
        stream.on("end", async () => {
          buffer = Buffer.concat(buffer);
          await sock.sendMessage(from, {
            audio: buffer,
            mimetype: "audio/mp4",
            fileName: `${title}.mp3`
          }, { quoted: msg });
        });
      } catch (e) {
        await sock.sendMessage(from, { text: "❌ Erro ao baixar áudio." }, { quoted: msg });
      }
    }
    break;

    case "ytmp4": {
      if (!args[0]) {
        return sock.sendMessage(from, { text: "❌ Envie o link do YouTube.\nExemplo: !ytmp4 https://youtu.be/xyz" }, { quoted: msg });
      }

      const url = args[0];
      if (!ytdl.validateURL(url)) {
        return sock.sendMessage(from, { text: "❌ Link inválido." }, { quoted: msg });
      }

      try {
        const info = await ytdl.getInfo(url);
        const title = info.videoDetails.title;

        const stream = ytdl(url, { quality: "18" }); // 18 = 360p com áudio
        let buffer = [];
        stream.on("data", chunk => buffer.push(chunk));
        stream.on("end", async () => {
          buffer = Buffer.concat(buffer);
          await sock.sendMessage(from, {
            video: buffer,
            caption: `🎬 ${title}`
          }, { quoted: msg });
        });
      } catch (e) {
        await sock.sendMessage(from, { text: "❌ Erro ao baixar vídeo." }, { quoted: msg });
      }
    }
    break;
