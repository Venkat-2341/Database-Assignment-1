% ═══════════════════════════════════════════════════════
% README / PROJECT OVERVIEW
% ═══════════════════════════════════════════════════════
\newpage
\section*{README — Olympia Track}
\addcontentsline{toc}{section}{README — Olympia Track}
\textcolor{accent}{\rule{\textwidth}{0.8pt}}
\vspace{0.3cm}

\subsection*{What is Olympia Track?}

\textbf{Olympia Track} is a relational database-backed Sports Management System designed for schools, colleges, and sports clubs. It replaces scattered spreadsheets and manual registers with a single, structured database that handles everything from tournament scheduling to player health records.

The system is built around \textbf{13 normalized tables}, covering 9 core entities and 4 junction/tracking tables. It enforces referential integrity via foreign keys, business rules via CHECK constraints, and uniqueness where required (e.g., sport names, member emails).

\vspace{0.4cm}
\subsection*{Project Structure}

\begin{table}[H]
\centering
\small
\renewcommand{\arraystretch}{1.3}
\begin{tabular}{l l p{8cm}}
\toprule
\textbf{File / Folder} & \textbf{Type} & \textbf{Description} \\
\midrule
\texttt{report.tex}        & LaTeX       & This document — full conceptual design report \\
\texttt{schema.sql}        & SQL         & \texttt{CREATE TABLE} statements for all 13 tables \\
\texttt{data.sql}          & SQL         & \texttt{INSERT} statements (15–20 rows per table) \\
\texttt{queries.sql}       & SQL         & Sample queries demonstrating system functionality \\
\texttt{ERDiag2.jpeg}      & Image       & ER diagram (dark-mode reference) \\
\texttt{UML class (2).svg} & SVG         & UML relationship diagram \\
\bottomrule
\end{tabular}
\caption*{Project file structure}
\end{table}

\vspace{0.3cm}
\subsection*{Core Entities at a Glance}

\begin{table}[H]
\centering
\small
\renewcommand{\arraystretch}{1.3}
\begin{tabular}{l l p{9cm}}
\toprule
\textbf{Entity} & \textbf{Key} & \textbf{Role in System} \\
\midrule
\texttt{Member}          & MemberID   & Players, coaches, and admins — the central actor \\
\texttt{Sport}           & SportID    & Defines sport types (Team / Individual / Dual) \\
\texttt{Team}            & TeamID     & Groups of members playing a sport, with coach and captain \\
\texttt{Venue}           & VenueID    & Physical facilities with capacity and surface info \\
\texttt{Tournament}      & TournamentID & High-level championship grouping events \\
\texttt{Event}           & EventID    & Individual matches/races within a tournament \\
\texttt{Equipment}       & EquipmentID & Gear inventory tracked per sport \\
\texttt{PracticeSession} & SessionID  & Scheduled team training at a venue \\
\texttt{PerformanceLog}  & LogID      & Timestamped player metrics (speed, goals, etc.) \\
\texttt{MedicalRecord}   & RecordID   & Injury and recovery tracking per member \\
\bottomrule
\end{tabular}
\caption*{Summary of core entities}
\end{table}

\newpage

\subsection*{How to Run}

\textbf{Prerequisites:} MySQL 8.0+ (or MariaDB 10.6+), any SQL client (MySQL Workbench, DBeaver, \texttt{mysql} CLI).

\vspace{0.3cm}
\noindent\textbf{Step 1 — Create the database and load schema:}
\begin{verbatim}
mysql -u root -p
CREATE DATABASE olympia_track;
USE olympia_track;
SOURCE schema.sql;
\end{verbatim}

\noindent\textbf{Step 2 — Load sample data:}
\begin{verbatim}
SOURCE data.sql;
\end{verbatim}

\noindent\textbf{Step 3 — Run sample queries:}
\begin{verbatim}
SOURCE queries.sql;
\end{verbatim}

\vspace{0.4cm}
\subsection*{Key Design Decisions}

\begin{enumerate}[leftmargin=1.5cm, itemsep=4pt]
    \item \textbf{Participation is Team-based, not Member-based.} For individual sports (e.g., Tennis Singles), a solo player registers as a "Team of One." This lets the same Participation table handle both team and individual events without schema changes.

    \item \textbf{Captaincy is a FK, not a flag.} Rather than an \texttt{IsCaptain} boolean scattered in the roster, \texttt{Team.CaptainID} points directly to one member, enforcing exactly one captain per team at the database level.

    \item \textbf{SQL keyword conflicts were renamed.} The UML attribute \texttt{Condition} was split into \texttt{EquipmentCondition} and \texttt{MedicalCondition}. \texttt{Rank} was renamed \texttt{EventRank}. This prevents reserved-word errors across all SQL dialects.

    \item \textbf{EquipmentIssue tracks both issue and return.} A single row stores \texttt{IssueDate}, \texttt{ReturnDate} (nullable), and \texttt{Quantity}, giving a complete audit trail per equipment loan without duplicate rows.
\end{enumerate}

\vspace{0.4cm}
\subsection*{Compliance Summary}

\begin{table}[H]
\centering
\small
\renewcommand{\arraystretch}{1.3}
\begin{tabular}{l c c}
\toprule
\textbf{Requirement} & \textbf{Minimum} & \textbf{Delivered} \\
\midrule
Distinct Entities          & $\geq 5$  & \textbf{9}  \\
Total Tables               & $\geq 10$ & \textbf{13} \\
Rows per Table             & 10–20     & \textbf{15–20} \\
NOT NULL columns per table & $\geq 3$  & \textbf{3–8} \\
Foreign Key Relationships  & —         & \textbf{14} \\
CHECK Constraints          & —         & \textbf{8}  \\
\bottomrule
\end{tabular}
\caption*{Assignment compliance at a glance}
\end{table}

\vspace{0.4cm}
\subsection*{Course Information}

\begin{tabular}{rl}
\textbf{Course:}     & CS 432 — Databases \\
\textbf{Assignment:} & Assignment 1 (Track 1) \\
\textbf{Semester:}   & II (2025--2026) \\
\textbf{Institute:}  & Indian Institute of Technology, Gandhinagar \\
\textbf{Instructor:} & Dr.\ Yogesh K.\ Meena \\
\end{tabular}
