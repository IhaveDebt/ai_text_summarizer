/**
 * AI Text Summarizer (Offline Simulation)
 *
 * Uses TF-IDF-like logic to summarize long text by selecting
 * the most relevant sentences based on keyword weighting.
 */

function summarize(text: string, numSentences: number = 3): string {
  const sentences = text.split(/[.!?]/).map(s => s.trim()).filter(Boolean);
  const words = text.toLowerCase().match(/\b\w+\b/g) || [];
  const freq: Record<string, number> = {};

  words.forEach(w => (freq[w] = (freq[w] || 0) + 1));
  const scores = sentences.map(s => {
    const wordsInSentence = s.toLowerCase().match(/\b\w+\b/g) || [];
    const score = wordsInSentence.reduce((sum, w) => sum + (freq[w] || 0), 0);
    return { sentence: s, score };
  });

  const top = scores.sort((a, b) => b.score - a.score).slice(0, numSentences);
  return top.map(s => s.sentence).join(". ") + ".";
}

if (require.main === module) {
  const text = `
    Artificial Intelligence is transforming industries. It helps automate tasks and extract insights from data.
    Developers are using machine learning to build recommendation engines, fraud detection systems, and chatbots.
    However, ethical AI use and transparency remain critical concerns for the future.
  `;
  console.log("Summary:\n", summarize(text, 2));
}
