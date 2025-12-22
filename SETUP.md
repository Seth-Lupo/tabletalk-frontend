# Table Talk Voice Demo Setup

This guide explains how to configure the live voice demo on the landing page.

---

## Quick Start

### 1. Create your ElevenLabs Agent

1. Go to [ElevenLabs Conversational AI](https://elevenlabs.io/app/conversational-ai)
2. Click **Create Agent**
3. Configure the agent:
   - **Name:** Casa Luna Host
   - **Voice:** Choose a warm female voice (e.g., "Rachel" or "Bella")
   - **System Prompt:** Copy the entire prompt from `AGENT_SYSTEM_PROMPT.md`
   - **First Message:** `Casa Luna, this is Luna speaking. How can I help you this evening?`

4. Save the agent and copy the **Agent ID** from the URL or settings

### 2. Configure Environment Variables

Create a `.env` file in the `frontend` directory:

```bash
cp .env.example .env
```

Edit `.env` and add your Agent ID:

```
PUBLIC_ELEVENLABS_AGENT_ID=your_actual_agent_id_here
```

> **Note:** The `PUBLIC_` prefix is required for SvelteKit to expose the variable to the client.

### 3. Add Background Ambience (Optional)

For immersive restaurant atmosphere during calls:

1. Download a restaurant ambience audio file (suggestions below)
2. Save it as `static/ambient-restaurant.mp3`

**Recommended free ambient sounds:**
- [Freesound.org](https://freesound.org/) - Search "restaurant ambience"
- [Mixkit](https://mixkit.co/free-sound-effects/restaurant/) - Free restaurant sounds
- [Pixabay](https://pixabay.com/sound-effects/) - Search "cafe ambience"

Look for:
- Subtle background chatter (not too loud)
- Soft clinking of glasses/dishes
- Gentle music in background
- ~2-5 minutes, loopable

### 4. Run the Development Server

```bash
npm run dev
```

Visit the site and click **Start Demo Call** to test the voice agent.

---

## File Structure

```
frontend/
├── .env                          # Your environment variables (create this)
├── .env.example                  # Template for env vars
├── AGENT_SYSTEM_PROMPT.md        # System prompt for ElevenLabs agent
├── SETUP.md                      # This file
├── static/
│   └── ambient-restaurant.mp3    # Optional background audio
└── src/
    └── lib/
        └── components/
            └── VoiceDemo.svelte  # The voice demo widget
```

---

## Agent Configuration Tips

### Voice Selection

For a Mexican restaurant host, consider:
- **Warm, friendly tone** - not too formal
- **Natural cadence** - conversational, not robotic
- **Moderate speed** - clear but not slow

### Testing Your Agent

Common test scenarios:
1. "I'd like to make a reservation for Saturday at 7pm, party of 4"
2. "Do you have any gluten-free options?"
3. "What's in the mole negro? I have a nut allergy"
4. "Can I order takeout?"
5. "What time do you close on Sundays?"

### Refining the Prompt

If the agent isn't responding correctly:
- Check the system prompt for missing information
- Add specific phrases for common edge cases
- Adjust temperature (lower = more consistent, higher = more varied)

---

## Troubleshooting

### "Agent ID not configured" error
- Make sure `.env` file exists in the `frontend` directory
- Ensure the variable is named `PUBLIC_ELEVENLABS_AGENT_ID`
- Restart the dev server after changing `.env`

### Microphone access denied
- Check browser permissions
- Ensure you're on `localhost` or `https://` (not `http://`)
- Try a different browser

### No audio from agent
- Check your system audio output
- Ensure the ElevenLabs agent has a voice configured
- Try refreshing the page

### Connection fails immediately
- Verify your Agent ID is correct
- Check that the agent is set to "public" in ElevenLabs
- Check browser console for detailed errors

---

## Production Considerations

### Security

For production deployments:

1. **Private Agents:** If your agent requires authentication, you'll need a backend endpoint to generate signed URLs. The current setup only works with public agents.

2. **Rate Limiting:** Consider adding rate limiting to prevent abuse.

3. **HTTPS:** The microphone API requires a secure context (HTTPS or localhost).

### Static Hosting

The current adapter is `adapter-static`. For the voice demo to work:
- The agent must be configured as **public** in ElevenLabs
- No server-side code is needed
- All authentication happens client-side

### Switch to Node Adapter

If you need server-side features (private agents, API proxying):

```bash
npm install @sveltejs/adapter-node
```

Update `svelte.config.js`:
```js
import adapter from '@sveltejs/adapter-node';
```

---

## Cost Considerations

ElevenLabs Conversational AI pricing:
- Check current pricing at [elevenlabs.io/pricing](https://elevenlabs.io/pricing)
- Demo usage counts against your quota
- Consider adding usage limits for the demo

---

## Support

- ElevenLabs Docs: https://elevenlabs.io/docs
- ElevenLabs Discord: Community support
- GitHub Issues: For Table Talk specific issues
