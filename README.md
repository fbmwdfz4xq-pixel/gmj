// 1. 严格的 Prompt 设计
const systemPrompt = `
You are a top-tier financial analyst. Analyze the given stock market data and provide an analysis.
CRITICAL INSTRUCTION: You MUST return ONLY a raw, valid JSON object. Do not include any conversational text, greetings, or markdown formatting (DO NOT use \`\`\`json).
The JSON MUST perfectly match this exact schema:
{
  "summary": "String (Short summary of the stock's current situation)",
  "sentiment": "Bullish" | "Neutral" | "Bearish",
  "risk_level": "High" | "Medium" | "Low"
}
`;

// 2. 调用 LLM 时的配置
const response = await openai.chat.completions.create({
    model: "gpt-3.5-turbo",
    response_format: { type: "json_object" }, // 强制开启 JSON 模式
    messages: [
        { role: "system", content: systemPrompt },
        { role: "user", content: `Here is the stock data for ${ticker}: ${JSON.stringify(marketData)}` }
    ],
    temperature: 0.2 // 降低随机性，确保格式稳定
});

