# 🎬 Multimodal Video Search Engine

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![PyTorch](https://img.shields.io/badge/PyTorch-2.0+-ee4c2c.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

An end-to-end semantic search system for video content that enables natural language queries over both spoken content and visual context. This project combines state-of-the-art speech recognition, vision models, and vector similarity search to create an intelligent video search engine.

## 🌟 Overview

This project implements a **multimodal search engine** that can index **any video content**—from YouTube lectures to Zoom recordings to internal company videos—and enables natural-language search across multiple modalities:
- **Audio**: Transcribes spoken content with precise timestamps
- **Visual**: Understands visual context through frame embeddings
- **Text**: Performs semantic search using transformer-based embeddings

Simply ask questions in natural language, and the system will find the most relevant moments in your videos based on what was said and what was shown.

## ✨ Key Features

- **📹 Universal Video Support**: Works with YouTube, local files, Zoom recordings, or any video source
- **🎙️ Audio Transcription**: Automatic speech-to-text using OpenAI Whisper with timestamp alignment
- **👁️ Visual Understanding**: Frame-level semantic analysis using CLIP (Vision Transformer)
- **🔍 Semantic Search**: Natural language queries powered by Sentence Transformers
- **⚡ Fast Retrieval**: Efficient similarity search using FAISS indexing
- **📊 Multimodal Fusion**: Combines audio and visual embeddings for comprehensive search
- **🎯 Timestamp Precision**: Returns exact time segments for matching content
- **🔄 Flexible Input**: Process videos from URLs, local files, or cloud storage

## 🛠️ Technology Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **Audio → Text** | OpenAI Whisper (base) | Transcribe speech with timestamps |
| **Visual Embeddings** | CLIP (ViT-B/32) | Encode frames into semantic vectors |
| **Text Embeddings** | Sentence-Transformers (all-MiniLM-L6-v2) | Encode transcripts for semantic search |
| **Vector Search** | FAISS (CPU) | Fast similarity search over embeddings |
| **Video Processing** | FFmpeg, yt-dlp | Download and process video content |

## 📋 Prerequisites

- Python 3.8 or higher
- FFmpeg installed on your system
- 4GB+ RAM recommended
- Internet connection (for downloading videos)



## 📊 System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Video Input                          │
│                  (YouTube URL / Local File)                 │
└────────────────────────┬────────────────────────────────────┘
                         │
        ┌────────────────┴────────────────┐
        │                                 │
        ▼                                 ▼
┌───────────────┐               ┌──────────────────┐
│ Audio Stream  │               │  Video Frames    │
│   (FFmpeg)    │               │  (1 fps extract) │
└───────┬───────┘               └────────┬─────────┘
        │                                │
        ▼                                ▼
┌───────────────┐               ┌──────────────────┐
│   Whisper     │               │   CLIP Model     │
│ Transcription │               │ (ViT-B/32)       │
└───────┬───────┘               └────────┬─────────┘
        │                                │
        ▼                                ▼
┌───────────────┐               ┌──────────────────┐
│     Text      │               │     Image        │
│  Embeddings   │               │   Embeddings     │
│  (MiniLM-L6)  │               │    (512-dim)     │
└───────┬───────┘               └────────┬─────────┘
        │                                │
        └────────────────┬───────────────┘
                         │
                         ▼
              ┌──────────────────┐
              │ Combined Vector  │
              │  (Text + Image)  │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │  FAISS Index     │
              │  (L2 Normalized) │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Search Results   │
              │ (Top-K Segments) │
              └──────────────────┘
```

## 💡 The Technical Challenge

### Why Video Search is Hard

While **text RAG** (Retrieval-Augmented Generation) is largely a solved problem, **video search** presents unique challenges:

1. **Multimodal Synchronization**: Aligning audio transcripts with visual frames requires precise timestamp coordination
2. **Unstructured Data**: Videos don't have inherent structure—you must create it through processing
3. **Computational Complexity**: Processing hours of video generates massive amounts of data
4. **Semantic Alignment**: Matching user intent across both spoken words and visual content
5. **Scale**: Enterprise video libraries can contain thousands of hours of content

### This Project's Solution

This implementation demonstrates handling these challenges through:
- **Robust Pipeline**: Automated video ingestion, processing, and indexing
- **Temporal Alignment**: Precise synchronization between transcript segments and video frames  
- **Efficient Indexing**: FAISS-based vector search for sub-second query times
- **Multimodal Fusion**: Combined text and visual embeddings for comprehensive search
- **Production-Ready**: Handles various video formats, resolutions, and content types

## 🎯 How It Works

1. **Video Acquisition**: Downloads YouTube video or uses local file
2. **Preprocessing**: 
   - Extracts audio at 16kHz for Whisper
   - Samples frames at 1 FPS for visual analysis
3. **Transcription**: Uses OpenAI Whisper to generate timestamped transcripts
4. **Embedding Generation**:
   - Text: Sentence-BERT creates semantic embeddings of transcript segments
   - Visual: CLIP generates embeddings for each extracted frame
5. **Alignment**: Synchronizes text segments with corresponding video frames
6. **Indexing**: Builds FAISS index from combined (text + visual) embeddings
7. **Search**: User queries are encoded and matched against the index using cosine similarity

## 📈 Performance

- **Processing Speed**: ~5 minutes of video processed in ~2-3 minutes (CPU)
- **Search Latency**: < 100ms for typical queries
- **Index Size**: ~10-15 MB per hour of video content
- **Accuracy**: High semantic relevance for natural language queries

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- [OpenAI Whisper](https://github.com/openai/whisper) for speech recognition
- [CLIP](https://github.com/openai/CLIP) for vision-language models
- [Sentence Transformers](https://www.sbert.net/) for text embeddings
- [FAISS](https://github.com/facebookresearch/faiss) for efficient similarity search



