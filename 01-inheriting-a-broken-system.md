Inheriting a Broken System: Why Single-Pass GPT-4 Tutoring Agents Hallucinate                                                                        
                                                                               
  It was June 2025. We had a mandate: build an AI tutoring agent that could answer student questions across multiple school curricula. There was no    
  existing system to build on, no foundation to work with. Just a blank canvas and eight weeks to ship something.                                      
   
  The choice seemed obvious at the time. GPT-4 was the most capable language model available, and the technical approach was straightforward—call the  
  API with a student's question, receive an answer, move on. Single-pass. Efficient. Deployable. We built it and launched it to over 5,000 students
  across different schools. By conventional metrics, it worked: response times were fast, API costs were reasonable, and students were receiving       
  answers to their questions.                               

  Then a bug report arrived that changed everything.                                                                                                   
  
  A student had asked about calculus—something involving derivatives. The specifics faded from memory, but the answer didn't. GPT-4 had generated a    
  response that was authoritative, well-organized, and completely false. The formula it produced didn't exist. A student who trusted the system enough
  to memorize it would fail their exam. And that's when the problem became something beyond a technical issue—it became an educational one.            
                                                            
  How Single-Pass GPT-4 Works (And Where It Breaks)                                                                                                    
  
  Understanding what we built requires understanding how it fails.                                                                                     
                                                            
  When you send a question to GPT-4, the model processes your prompt through neural networks and generates the most statistically probable next        
  tokens—one at a time. It's generating based on patterns learned during training. It isn't searching a knowledge base. It isn't reasoning from a
  structured system. It's predicting what text should come next based on statistical patterns in data it's seen before.                                
                                                            
  This approach works well for many tasks. Drafting emails, explaining familiar concepts, generating text in established patterns—GPT-4 excels at      
  these. But education operates differently. When students ask about a specific curriculum—a particular textbook, a specific chapter, a formula in a
  particular format—the model is still just predicting probable continuations. If its training data included similar questions, it guesses correctly.  
  If not, it hallucinates. It generates plausible-sounding answers because that's what statistical patterns predict when grounded knowledge is absent.

  It's like asking someone who's read extensively about calculus to answer a calculus question without access to those books. They might answer        
  correctly if they remember. They might also confidently invent a formula that sounds plausible but is entirely wrong.
                                                                                                                                                       
  Why Hallucinations Matter in Education                    

  What makes this particularly dangerous in a tutoring context is that students operate from a position of trust. A high schooler using an AI tutor    
  assumes the answers are correct. They aren't fact-checking GPT-4's calculus—they're learning from it. If the system hallucinates once, maybe a
  student notices the error. If it happens twice without detection? A student has internalized something false, built their understanding on faulty    
  foundations.                                              

  This isn't merely a user experience problem. It's a product failure. The entire value of an AI tutor is its reliability. A human tutor can make      
  mistakes too, but students can push back—"Wait, that doesn't sound right"—and trigger reconsideration. GPT-4 doesn't self-correct. It commits to its
  hallucinations with the same confidence it would use for a verified fact.                                                                            
                                                            
  By the time that bug report reached us, we'd deployed to thousands of students. How many incorrect answers had they already memorized? How many would
   fail exams because they'd learned something false? We didn't know. That uncertainty was the real crisis.
                                                                                                                                                       
  The Pivot Point                                                                                                                                      
  
  I remember sitting with the team, looking at that calculus response, and recognizing something had broken in a way that mattered more than speed or  
  cost. We'd built something fast. We'd shipped something that appeared to work. But we'd shipped something fundamentally broken in an educational
  context.                                                                                                                                             
                                                            
  We had July and August left.

  The question shifted from "How do we optimize single-pass GPT-4?" to "How do we fundamentally redesign this so hallucinations stop being possible?"  
  
  Someone mentioned RAG—Retrieval-Augmented Generation. The idea was straightforward: instead of generating from statistical patterns alone, provide   
  the model with actual source material first. Ground answers in real curriculum data. Make hallucinations structurally impossible rather than merely
  unlikely.                                                                                                                                            
                                                            
  We didn't know if we could pull it off in eight weeks. We didn't know if the architecture would scale to 5,000 concurrent students. We didn't even   
  know if we had the right tools—Chroma, Langsmith, the emerging stack was unfamiliar territory.
                                                                                                                                                       
  But we knew single-pass GPT-4 wasn't the answer. And in that calculus hallucination, we had proof.                                                   
  
